# Xsd2Rb - a RubyGem to simplify and automate XML generation in Ruby
## Summary
This readme covers the xsd2rb project, originally written by Chris To (https://github.com/chrt00) in April 2017. The code is not present in this repo as it was written for Jane Inc. https://github.com/janeapp for their Telus eClaims integration. The three month time investment used to write this library helped automate 100,000's of lines in code changes. This document is not comprehensive or 100% accurate as I am recalling from memory.

![20220816_213526](https://user-images.githubusercontent.com/326725/185209678-6d943ad2-581d-40da-8acc-dced77078955.png)

## Motivation
The Telus eClaims insurance integration process began with using Savon https://github.com/savonrb/savon, a gem that used a hash-based DSL to generate XML. However, due the complexity of the HL7 XML standard (https://www.hl7.org/implement/standards/product_brief.cfm?product_id=185), it was impossible to generate correct SOAP XML after a few levels of nesting. This required a new solution to generate XML in Ruby.

Using Java or other languages with mature XML toolchains was not an option due to the small team not having any experience or logistics to use anything but Ruby.

Generation of the SOAP envelopes originally was done manually via libXML/nokogiri. Much time was spent with US tabloid size printouts to cover the 300+ page spec of eClaims.

Eventually, I experimented with Ruby metaprogramming to dynamically generate these classes in memory; however without any code to analyize it was too difficult to verify correctness. Henceforth the xsd2rb gem was born.

The goals were to:
- Save time and effort from laborious and error prone process of manually generating XML from a given XML schema, in Ruby
- Design a consistent pattern to generate XML objects in Ruby
- Automate validation of XML generation
- Reduce and eliminate human error from transcribing manually
- Provide an extensible design to allow user defined mixins
  - Allow schema updates to not overwrite user-written code

## Design

The gem is meant to be used via command line like:
`xsd2rb MODULE_NAME MY_XSD_FOLDER`
Running the command would output Ruby files in a folder like `FOLDER_NAME_output`

The contents of the folder would have a single file to include all the generated code and also link the mixins, with the intent that these code files would be transferred to its own rubygem.

The code would execute like: 
- Scan directory for schema files, per schema file:
  - Parse xsd
  - Read fields and child nodes, generating objects containing validation and structure information
  - Generate Xsd2Rb::XmlNode from generated object list above, defining:
    - basic validations
    - initializer parameters
    - class structure pattern

### Xsd2Rb::XmlNode pattern

#### Validations
Things that were validated automatically:
- Primitive types (https://www.w3.org/TR/xmlschema-2/#built-in-primitive-datatypes)
  - Only a partial subset was implemented. 
- Child nodes
  - Valid child node types according to schema
  - Minimum/maximum occurance of each node

#### Mixin Callbacks
A hook would be called in the validation phase. The call would be defined in the mixin; usually the method call would be an empty method (emulating an interface pattern in Java). Tthe plan was to allow for addition of more callbacks as Xsd2Rb was extended and improved.
  
#### Mixins
Mixins were meant to be included in the automatically generated classes.
The implenentation at the time I believe was a crude monkeypatch solution, that I would have done differently given more time.

I believe the implementation at the time was that there was an `Xsd2Rb::Mixin` class that implemented an interface, and the automatically generated class would have an internal class defined like:

```
module MODULE_NAME
  class XSD_SCHEMA_NAME
    ...
    class Mixin < Xsd2Rb::Base
       ...
    end
  end
end
```
Any user written callbacks in the `XSD_MODULE_NAME::XSD_SCHEMA_NAME::Mixin` monkeypatch would then be run.

### xsd parser
Thi

### code generation

## Testing
Generation of practical SOAP envelopes from the eClaims project was used to test XML and code correctness.

## Caveats
- DSL was not 100% XML standard compliant - some xml attribute features unsupported
- Monkeypatch pattern should be changed

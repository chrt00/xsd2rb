# xsd2rb - a rubygem to simplify and automate XML generation in Ruby
## Summary
This readme covers the xsd2rb project, originally written by Chris To (https://github.com/chrt00) in April 2017. The code is not present in this repo as it was written for Jane Inc. https://github.com/janeapp for their Telus eClaims integration. The three month time investment used to write this library helped automate 100,000's of code changes. This document is not comprehensive as I am recalling from memory.

## Motivation
The Telus eClaims insurance integration process began with using Savon https://github.com/savonrb/savon, a gem that used a hash-based DSL to generate XML. However, due the complexity of the HL7 XML standard, it was impossible to generate correct SOAP XML after a few levels of nesting. This required a new solution to generate XML in Ruby.

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

### xsd2rb XML node pattern

#### Validations
Things that were validated automatically:
- Primitive types (https://www.w3.org/TR/xmlschema-2/#built-in-primitive-datatypes)
  - Only a partial subset was implemented. 
- Child nodes
  - Valid child node types according to schema
  - Minimum/maximum occurance of each node
  
#### Mixins

### xsd parser

### code generation

## Testing

## Caveats
- DSL was not 100% XML standard compliant - some xml attribute features unsupported

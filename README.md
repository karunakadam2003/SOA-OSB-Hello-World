# SOA-OSB-Hello-World


---

BPEL Process (HelloWorld.bpel)

<?xml version="1.0" encoding="UTF-8"?>
<process name="HelloWorld"
         targetNamespace="http://xmlns.oracle.com/HelloWorld"
         xmlns="http://docs.oasis-open.org/wsbpel/2.0/process/executable"
         xmlns:client="http://xmlns.oracle.com/HelloWorld"
         xmlns:xsd="http://www.w3.org/2001/XMLSchema"
         xmlns:ns1="http://xmlns.oracle.com/HelloWorld"
         xmlns:bpws="http://docs.oasis-open.org/wsbpel/2.0/process/executable"
         xmlns:ora="http://schemas.oracle.com/xpath/extension">
  
  <partnerLinks>
    <partnerLink name="client"
                 partnerLinkType="client:HelloWorld"
                 myRole="HelloWorldProvider"/>
  </partnerLinks>

  <variables>
    <variable name="input" messageType="client:HelloWorldRequestMessage"/>
    <variable name="output" messageType="client:HelloWorldResponseMessage"/>
  </variables>

  <sequence name="main">
    <receive name="receiveInput" partnerLink="client" portType="client:HelloWorld"
             operation="process" variable="input" createInstance="yes"/>
    
    <assign name="AssignHello">
      <copy>
        <from expression="concat('Hello, ', $input.payload/client:name)"/>
        <to variable="output" part="payload" query="/client:greeting"/>
      </copy>
    </assign>

    <reply name="replyOutput" partnerLink="client" portType="client:HelloWorld"
           operation="process" variable="output"/>
  </sequence>
</process>


---

WSDL (HelloWorld.wsdl)

<definitions xmlns="http://schemas.xmlsoap.org/wsdl/"
             xmlns:tns="http://xmlns.oracle.com/HelloWorld"
             xmlns:xs="http://www.w3.org/2001/XMLSchema"
             targetNamespace="http://xmlns.oracle.com/HelloWorld">
  <types>
    <xs:schema targetNamespace="http://xmlns.oracle.com/HelloWorld">
      <xs:element name="HelloWorldRequest">
        <xs:complexType>
          <xs:sequence>
            <xs:element name="name" type="xs:string"/>
          </xs:sequence>
        </xs:complexType>
      </xs:element>
      <xs:element name="HelloWorldResponse">
        <xs:complexType>
          <xs:sequence>
            <xs:element name="greeting" type="xs:string"/>
          </xs:sequence>
        </xs:complexType>
      </xs:element>
    </xs:schema>
  </types>

  <message name="HelloWorldRequestMessage">
    <part name="payload" element="tns:HelloWorldRequest"/>
  </message>
  <message name="HelloWorldResponseMessage">
    <part name="payload" element="tns:HelloWorldResponse"/>
  </message>

  <portType name="HelloWorld">
    <operation name="process">
      <input message="tns:HelloWorldRequestMessage"/>
      <output message="tns:HelloWorldResponseMessage"/>
    </operation>
  </portType>
</definitions>


---

What to do next:

Paste this BPEL code into Copilot and ask:

> Convert this SOA HelloWorld BPEL service into a Java Spring Boot REST service that takes a name and returns Hello, name.



# EJERCICIOS
## ejercicio parte 1
Parte 1. Crear un parseador DOM:
```java
package com.nicolas.practicatema3;

import java.io.File;
import java.io.IOException;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;
import org.w3c.dom.Document;
import org.xml.sax.SAXException;

public class PracticaTema3 {

    public static void main(String[] args) {
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        factory.setValidating(true);
        factory.setIgnoringElementContentWhitespace(true);

        try {
            DocumentBuilder builder = factory.newDocumentBuilder();
            File file = new File("XML_document.xml");
            Document doc = builder.parse(file);
            doc.getDocumentElement().normalize();
        } catch (ParserConfigurationException | SAXException | IOException e) {
            e.printStackTrace();
        }
    }
}
```
## ejercicio parte 2
Parte 2. Procesamiento de un fichero XML haciendo uso de un parseador DOM:
```java 
public class PracticaTema3 {

    public static void main(String[] args) throws XPathExpressionException {
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        factory.setValidating(true);
        factory.setIgnoringElementContentWhitespace(true);

        try {
            DocumentBuilder builder = factory.newDocumentBuilder();
            File file = new File("XML_document.xml");
            Document doc = builder.parse(file);
            doc.getDocumentElement().normalize();
            
            XPath xPath = XPathFactory.newInstance().newXPath();
            String expression = "/class/student";
            NodeList nodeList = (NodeList) xPath.compile(expression).evaluate(doc, XPathConstants.NODESET);
            
            
            for(int i = 0; i < nodeList.getLength();i++){
                Node nNode = nodeList.item(i);
                System.out.println("\nCurrent Element:" + nNode.getNodeName());
                Element eElement = (Element) nNode;
                System.out.println("Student roll no:" + eElement.getAttribute("rollno"));
                
                System.out.println("First Name:" + eElement
                        .getElementsByTagName("firstname")
                        .item(0)
                        .getTextContent());
            }
        } catch (ParserConfigurationException | SAXException | IOException e) {
            e.printStackTrace();
        }

        
    }
}
```
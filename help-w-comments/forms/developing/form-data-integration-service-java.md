---
title: Form Data Integration Service JavaAPI Quick Start(SOAP)
seo-title: Form Data Integration Service JavaAPI Quick Start(SOAP)
description: null
seo-description: null
uuid: 539a4725-1134-4d55-b95e-221607ddf586
contentOwner: admin
content-type: reference
products: SG_EXPERIENCEMANAGER/6.4/FORMS
topic-tags: develop
discoiquuid: 1ce11dc0-9404-4d20-8507-6075d6a8ef73
index: y
internal: n
snippet: y
---

# Form Data Integration Service JavaAPI Quick Start(SOAP){#form-data-integration-service-javaapi-quick-start-soap}

The following Quick Starts are available for the Form Data Integration service.

[Quick Start (SOAP mode): Importing form data using the Java API](form-data-integration-service-java#quick_start_soap_mode_importing_form_data_using_the_java_api)

[Quick Start (SOAP mode): Exporting form data using the Java API](form-data-integration-service-java#quick_start_soap_mode_exporting_form_data_using_the_java_api)

AEM Forms operations can be performed using the AEM Forms strongly-typed API and the connection mode should be set to SOAP.

***Note**: Quick Start located in Programming with AEM forms are based on the Forms Server being deployed on JBoss Application Server and the Microsoft Windows operating system. However, if you are using another operating system, such as UNIX, replace Windows-specific paths with paths that are supported by the applicable operating system. Likewise, if you are using another J2EE application server, ensure that you specify valid connection properties. (See [Setting connection properties](/programming-with-aem-forms/invoking-aem-forms-using-java#setting_connection_properties).)*

## Quick Start (SOAP mode): Importing form data using the Java API {#quick-start-soap-mode-importing-form-data-using-the-java-api}

The following Java code example imports data into a PDF form. The data is located in an XML file named *Loan_data.xml* and the PDF form is saved as a PDF file named *ResultLoanForm.pdf*. (See [Importing Form Data](/programming-with-aem-forms/importing-exporting-data#importing_form_data).)

```as3
 /* 
     * This Java Quick Start uses the SOAP mode and contains the following JAR files 
     * in the class path: 
     * 1. adobe-formdataintegration-client.jar 
     * 2. adobe-livecycle-client.jar 
     * 3. adobe-usermanager-client.jar 
     * 4. adobe-utilities.jar 
     * 5. jboss-client.jar (use a different JAR file if the forms server is not deployed 
     * on JBoss) 
     * 6. activation.jar (required for SOAP mode) 
     * 7. axis.jar (required for SOAP mode) 
     * 8. commons-codec-1.3.jar (required for SOAP mode) 
     * 9.  commons-collections-3.1.jar  (required for SOAP mode) 
     * 10. commons-discovery.jar (required for SOAP mode) 
     * 11. commons-logging.jar (required for SOAP mode) 
     * 12. dom3-xml-apis-2.5.0.jar (required for SOAP mode) 
     * 13. jaxen-1.1-beta-9.jar (required for SOAP mode) 
     * 14. jaxrpc.jar (required for SOAP mode) 
     * 15. log4j.jar (required for SOAP mode) 
     * 16. mail.jar (required for SOAP mode) 
     * 17. saaj.jar (required for SOAP mode) 
     * 18. wsdl4j.jar (required for SOAP mode) 
     * 19. xalan.jar (required for SOAP mode) 
     * 20. xbean.jar (required for SOAP mode) 
     * 21. xercesImpl.jar (required for SOAP mode) 
     * 
     * These JAR files are located in the following path: 
     * <install directory>/sdk/client-libs/common 
     * 
     * The adobe-utilities.jar file is located in the following path: 
     * <install directory>/sdk/client-libs/jboss 
     * 
     * The jboss-client.jar file is located in the following path: 
     * <install directory>/jboss/bin/client 
     * 
     * SOAP required JAR files are located in the following path: 
     * <install directory>/sdk/client-libs/thirdparty 
     * 
     * If you want to invoke a remote forms server instance and there is a 
     * firewall between the client application and the server, then it is  
     * recommended that you use the SOAP mode. When using the SOAP mode,  
     * you have to include these additional JAR files 
     * 
     * For information about the SOAP  
     * mode, see "Setting connection properties" in Programming  
     * with AEM Forms 
     */ 
 import java.util.*; 
 import java.io.File; 
 import java.io.FileInputStream; 
 import com.adobe.idp.Document; 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory; 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties; 
 import com.adobe.livecycle.formdataintegration.client.*; 
  
 public class ImportDataSOAP { 
  
     public static void main(String[] args) { 
          
         try{ 
             //Set connection properties required to invoke AEM Forms using SOAP mode                                 
             Properties connectionProps = new Properties(); 
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "http://[server]:[port]"); 
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);           
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss"); 
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator"); 
             connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password"); 
              
              //Create a ServiceClientFactory object 
              ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps); 
                  
             //Create a FormDataIntegrationClient object 
             FormDataIntegrationClient dataClient = new FormDataIntegrationClient(myFactory);  
              
             //Import XDP XML data into an XFA PDF document 
             //Reference an XFA PDF form 
             FileInputStream inputStream = new FileInputStream("C:\\Adobe\Loan.pdf");  
             Document inputPDF = new Document(inputStream);  
              
             FileInputStream dataInput = new FileInputStream("C:\\Adobe\Loan_data.xml");  
             Document inputDataFile = new Document(dataInput);  
                                               
             //Import data into the form 
             Document resultPDF = dataClient.importData(inputPDF,inputDataFile); 
                  
             //Save the PDF file 
             File resultFile = new File("C:\\Adobe\ResultLoanForm.pdf");  
             resultPDF.copyToFile(resultFile); 
                  
         }catch (Exception e) { 
              e.printStackTrace(); 
         }     
       } 
     } 
 
```

## Quick Start (SOAP mode): Exporting form data using the Java API {#quick-start-soap-mode-exporting-form-data-using-the-java-api}

The following Java code example exports data from a PDF form. The form data is saved as an XML file named *Loan_data.xml*. (See [Exporting Form Data](/programming-with-aem-forms/importing-exporting-data#exporting_form_data).)

```as3
 /* 
     * This Java Quick Start uses the SOAP mode and contains the following JAR files 
     * in the class path: 
     * 1. adobe-formdataintegration-client.jar 
     * 2. adobe-livecycle-client.jar 
     * 3. adobe-usermanager-client.jar 
     * 4. adobe-utilities.jar 
     * 5. jboss-client.jar (use a different JAR file if the forms server is not deployed 
     * on JBoss) 
     * 6. activation.jar (required for SOAP mode) 
     * 7. axis.jar (required for SOAP mode) 
     * 8. commons-codec-1.3.jar (required for SOAP mode) 
     * 9.  commons-collections-3.1.jar  (required for SOAP mode) 
     * 10. commons-discovery.jar (required for SOAP mode) 
     * 11. commons-logging.jar (required for SOAP mode) 
     * 12. dom3-xml-apis-2.5.0.jar (required for SOAP mode) 
     * 13. jaxen-1.1-beta-9.jar (required for SOAP mode) 
     * 14. jaxrpc.jar (required for SOAP mode) 
     * 15. log4j.jar (required for SOAP mode) 
     * 16. mail.jar (required for SOAP mode) 
     * 17. saaj.jar (required for SOAP mode) 
     * 18. wsdl4j.jar (required for SOAP mode) 
     * 19. xalan.jar (required for SOAP mode) 
     * 20. xbean.jar (required for SOAP mode) 
     * 21. xercesImpl.jar (required for SOAP mode) 
     * 
     * These JAR files are located in the following path: 
     * <install directory>/sdk/client-libs/common 
     * 
     * The adobe-utilities.jar file is located in the following path: 
     * <install directory>/sdk/client-libs/jboss 
     * 
     * The jboss-client.jar file is located in the following path: 
     * <install directory>/jboss/bin/client 
     * 
     * SOAP required JAR files are located in the following path: 
     * <install directory>/sdk/client-libs/thirdparty 
     * 
     * If you want to invoke a remote forms server instance and there is a 
     * firewall between the client application and the server, then it is  
     * recommended that you use the SOAP mode. When using the SOAP mode,  
     * you have to include these additional JAR files 
     * 
     * For information about the SOAP  
     * mode, see "Setting connection properties" in Programming  
     * with AEM Forms 
     */ 
 import java.util.*; 
 import java.io.File; 
 import java.io.FileInputStream; 
 import com.adobe.idp.Document; 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactory; 
 import com.adobe.idp.dsc.clientsdk.ServiceClientFactoryProperties; 
 import com.adobe.livecycle.formdataintegration.client.*; 
  
 public class ExportDataSOAP { 
  
     public static void main(String[] args) { 
          
     try{ 
         //Set connection properties required to invoke AEM Forms using SOAP mode                                 
         Properties connectionProps = new Properties(); 
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_DEFAULT_SOAP_ENDPOINT, "http://[server]:[port]"); 
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_TRANSPORT_PROTOCOL,ServiceClientFactoryProperties.DSC_SOAP_PROTOCOL);           
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_SERVER_TYPE, "JBoss"); 
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_USERNAME, "administrator"); 
         connectionProps.setProperty(ServiceClientFactoryProperties.DSC_CREDENTIAL_PASSWORD, "password"); 
              
          //Create a ServiceClientFactory object 
          ServiceClientFactory myFactory = ServiceClientFactory.createInstance(connectionProps); 
                  
          //Create a FormDataIntegrationClient object 
          FormDataIntegrationClient dataClient = new FormDataIntegrationClient(myFactory);  
                  
          //Reference a PDF form from which to export data         
          FileInputStream fileInputStream2 = new FileInputStream("C:\\Adobe\LoanForm.pdf");  
          Document inputPDF = new Document(fileInputStream2);  
                                   
          //Export data from the form 
          Document resultPDF = dataClient.exportData(inputPDF); 
                  
          //Save the exported form data as an XML file 
          File resultFile = new File("C:\\Adobe\Loan_data.xml");  
          resultPDF.copyToFile(resultFile); 
                  
     }catch (Exception e) { 
              e.printStackTrace(); 
         }     
     } 
 }
```

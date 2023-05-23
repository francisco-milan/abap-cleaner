# Known installation issues 

## Error "Unable to read repository ... PKIX path building failed"

**Symptom**: After copying the URL https://sap.github.io/abap-cleaner/updatesite into 
Eclipse (menu 'Help / Install New Software', field 'Work with'), the error "Unable to read repository at https://sap.github.io/abap-cleaner/updatesite/content.xml"
is shown with details "PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: 
unable to find valid certification path to requested target" (or similar). 

**Solution**: Open the file ```eclipse.ini``` from your Eclipse installation path. 
Right after the line ```-vmargs```, add the following two lines: 

```
-Djavax.net.ssl.trustStore=NONE
-Djavax.net.ssl.trustStoreType=Windows-ROOT
```

Now save ```eclipse.ini```, restart Eclipse and try menu 'Help / Install New Software' again.

**Explanation**: Some component in your network (antivirus, firewall, corporate proxy etc.) is hooking into encrypted 
TLS connections. The fake certificate of this component needs to be trusted by the operating system / application.
Typically, this is done by injecting a trusted root certificate into the Windows operating system trust store. 
However, Eclipse, in its default configuration, does not make use of the Windows OS trust store, but uses the trust store 
of the Java VM. The above solution makes Eclipse use the Windows OS trust store. 

[**Back to README**](../README.md#requirements-and-installation)
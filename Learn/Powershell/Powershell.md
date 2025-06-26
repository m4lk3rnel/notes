
```powershell
Select-Object -Property
```

- web requests:
```powershell
Invoke-WebRequest -Uri "http://example.org" 
```

- word definitions from `https://dexonline.ro`:
```powershell
Invoke-WebRequest -Uri "https://dexonline/definitie/corelatie/json" | ConvertFrom-Json | Select-Object -Property definitions | Format-List *
```

- full details of a property.
```powershell
Select-Object -ExpandProperty 
```

```powershell
Invoke-WebRequest -Uri "https://dexonline.ro/definitie/corelatie/json" | ConvertFrom-Json | Select-Object -ExpandProperty definitions | Select-Object -ExpandProperty internalRep
```

---

- check sha512 sum of a file:
```powershell
certutil -hashfile "filename.exe" SHA512
```
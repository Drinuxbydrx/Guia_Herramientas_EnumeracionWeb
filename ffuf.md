# Ffuf

![](imagenes/img5.jpeg)

<h2 id="directorios"> 1.-Directorios y Archivos</h2>

Directorios con extensiones<br>
```sh
ffuf -w users.txt:FUZZ -w passwords.txt:FUZZ -X POST -d "username=FUZZ&password=FUZZ" -u http://example.com/login
```
```sh
ffuf -u https://example.com/FUZZ -w /usr/share/wordlists/dirb/common.txt -k
```

```sh
ffuf -u https://example.com/FUZZ -w /usr/share/wordlists/dirb/common.txt -k -fs 2407
```
```sh
ffuf -u https://example.com/FUZZ -w /usr/share/wordlists/dirb/common.txt -e .php,.txt,.bak -k
```

<h2 id="subdominios"> 2.-Subdominios</h2>

```sh
ffuf -u http://example.local -H "Host: FUZZ.target.local" -w /usr/share/wordlists/amass/subdomains.txt
```

<h2 id="filtros"> 3.-Filtros de Respuesta</h2>

```sh
ffuf -u http://example.com -w list.txt -fs 4242
```

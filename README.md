# NginxServer
<p> Colocando um servidor Nginx no ar, utilizando a WSL2 e Ubuntu 20.04 no Windows Subsystem for Linux e criação de script de status do serviço Nginx.</p>

# :pencil:DOCUMENTAÇÃO 

 - [Step 1: Ativando o subsistema do windows para linux.](https://github.com/Yyfii/NginxServer/blob/main/README.md#step-1-ativando-o-subsistema-do-windows-para-linux)
 - [Step 2: Baixar o Ubuntu dentro da Windows Store.](https://github.com/Yyfii/NginxServer/blob/main/README.md#step-2-baixar-o-ubuntu-dentro-da-windows-store)
 - [Step 3: Instalando o Nginx.](https://github.com/Yyfii/NginxServer/blob/main/README.md#step-3-instalando-o-nginx)
 - [Step 4: Configurando o Firewall.](https://github.com/Yyfii/NginxServer/blob/main/README.md#step-4-configurando-o-firewall)
 - [Step 5: Verificando o Nginx.](https://github.com/Yyfii/NginxServer/blob/main/README.md#step-5-verificando-o-nginx)
 - [Step 6: Criando o script de validação.](https://github.com/Yyfii/NginxServer/blob/main/README.md#step-6-criando-o-script-de-valida%C3%A7%C3%A3o)
 - [Step 7: Automatização do script shell com o cron.](https://github.com/Yyfii/NginxServer/blob/main/README.md#step-7-automatiza%C3%A7%C3%A3o-do-script-shell-com-o-cron)
 - [Step 8: Redirecionando para duas saídas (situações 1 - online 2 - offline)](https://github.com/Yyfii/NginxServer/blob/main/README.md#step-8-redirecionando-para-duas-sa%C3%ADda-situa%C3%A7%C3%B5es-1---online-2---offline)
 - [Step 9: Colocando o versão do script na mensagem.](https://github.com/Yyfii/NginxServer/blob/main/README.md#step-9-colocando-o-vers%C3%A3o-do-script-na-mensagem)
 - [DESCRIÇÃO DOS PRINCIPAIS COMANDOS](https://github.com/Yyfii/NginxServer/blob/main/README.md#descri%C3%A7%C3%A3o-dos-principais-comandos)
 - [REFERÊNCIAS](https://github.com/Yyfii/NginxServer/blob/main/README.md#referencias)

> Primeiramente iremos preparar o ambiente de trabalho, instalando as ferramentas necessárias :point_down:
>>`WSL2`
 `Ubuntu 20.04`
 `Windows 10`
 `Nginx`
 `Shell Script`
 `Cron`

  # Step 1: Ativando o subsistema do windows para linux.
     - Abra o painel de controle no windows.
     - Vá em programas.
     
   ![painel de controle](https://github.com/Yyfii/NginxServer/blob/main/WhatsApp%20Image%202024-10-25%20at%2009.22.53.jpeg)
   ![painel 2](https://github.com/Yyfii/NginxServer/blob/main/WhatsApp%20Image%202024-10-25%20at%2009.23.25.jpeg)

     - Clique em ativar e desativar recursos do windows.
     - Irá aparecer uma lista, procure "Subsistema do Windows para Linux".
     
   ![painel 3](https://github.com/Yyfii/NginxServer/blob/main/WhatsApp%20Image%202024-10-25%20at%2009.24.05.jpeg)
   
     - Ative a opção.
     - Dê o OK e espere a instalação finalizar.
     - Reinicie seu dispositivo.
     - Se o seu já estiver ativado, continue seguindo os próximos passos*.
  # Step 2: Baixar o Ubuntu dentro da Windows Store.
     - Vá no menu Iniciar do Windows.
     - Acessa a Microsoft Store.
     - Digite no campo de busca "Linux".
     - Procure a versão mais atual do Ubuntu LTS(Long Term Support).
     - Prossiga e Instale.
     - Vá no menu iniciar e ele irá aparecer.
   ![ubuntu](https://github.com/Yyfii/NginxServer/blob/main/search_ubuntu_iniciar.png)
   ![ubuntu](https://github.com/Yyfii/NginxServer/blob/main/searchBarUbuntu.png)

     - Clique nele.
     - Ao carregar ele irá pedir um nome de usuário.
     - E também pedirá uma senha. (*Tenha certeza de anotá-la em algum lugar seguro para não esquecê-la)
  # Step 3: Instalando o Nginx.
     - Antes de instalarmos qualquer coisa, iremos atualizar os pacotes.
     - Digite "sudo apt update && sudo apt upgrade".
   ![update](https://github.com/Yyfii/NginxServer/blob/main/updatePackages.png)
   
     - Se ele fizer alguma pergunta digite "y" para confirmar a instalação dos pacotes.
     - Agora digite "sudo apt install nginx"
     - Se ele fizer alguma pergunta digite "y" para confirmar a instalação dos pacotes.
     
   ![nginx install](https://github.com/Yyfii/NginxServer/blob/main/installNginx.png)

> Resumo dos comandos:
```sh
#> sudo apt update && sudo apt upgrade
#> sudo apt install nginx
```
   
  # Step 4: Configurando o Firewall.
     - Antes de testar, precisamos ter certeza de que o firewall não irá tentar impedir a ativação do nginx.
     - Listando as configurações das aplicações: "sudo ufw app list"
   ![status ufw](https://github.com/Yyfii/NginxServer/blob/main/firewallStatus.png)
   
       - Nginx Full ( Abre a porta 80 (tráfico web não criptografado) e 443 (TLS/SSL tráfico criptografado)
       - Nginx HTTP ( Abre apenas a porta 80)
       - Nginx HTTPS (Abre apenas a porta 443)
    - Por agora iremos ativar apenas o tráfico na porta 80, ou seja o Nginx HTTTP
    - Digite "sudo ufw allow 'Nginx HTTP'
    - Digite "sudo ufw disable" e em seguida "sudo ufw enable" para reiniciar o firewall.
    - Verfique o status: "sudo ufw status"
  ![status ufw](https://github.com/Yyfii/NginxServer/blob/main/firewallConfig.png)

> Resumo dos comandos:
```sh
#> sudo ufw app list
#> sudo ufw allow 'Nginx HTTP'
#> sudo ufw disable
#> sudo ufw enable
#> sudo ufw status
```
  # Step 5: Verificando o Nginx.
    - Podemos checar se ele está rodando com o comando "systemctl status nginx"
    
 ![nginx status](https://github.com/Yyfii/NginxServer/blob/main/statusNginxpt1.png)
 
    - - Comandos usados no nginx:
      - sudo systemctl stop nginx (para o serviço)
      - sudo systemctl systemctl start nginx (inicia o serviço)
      - sudo systemctl restart nginx (reiniciar o serviço)
      - sudo systemctl reload nginx (recarregar  o serviço sem parar as conexões)
      - sudo systemctl disable nginx (desabilitar o serviço)
      - sudo systemctl enable nginx (habilitar o serviço)

> Resumo dos comandos:
```sh
#> systemctl status nginx
```
  # Step 6: Criando o script de validação
  
  > Iremos utilizar um shell script(irei chamá-lo de "status.sh"), com uma extensão .sh, esse script validará se o serviço está ativo ou não e enviaá o seu status para um diretório que iremos criar(irei chamá-lo de "StatusNginx").
  > O status.sh conterá Data e hora, nome do serviço,  status, uma mensagem personalizada indicando seu status( online ou offline).
  O status.sh gerará dois arquivos de saída:
  - online.sh - arquivo de serviço online.
  - offline.sh - arquivo de serviço offline.
  > Esse script será executado a cada 5 minutos, e a cada execução ele terá uma nova versão.(Ex: statusv1.sh)
  
  - `História do script`: Se o nginx aparecer quando pesquisarmos todos os serviços ativos do sistema -> ele está rodando, e iremos executar o script online.sh e mandar a execução para o arquivo de log para que possa servir como um histórico de todas as execuções. Se ele não aparecer, será executado o arquivo offline.sh e mandado a ocorrencia para o arquivo de log, o status.log.
  
  Primeiramente iremos criar um diretorio:
  - Logue como root "su root"
  - Entre no diretorio raiz "cd / "
  - Criar o diretório "mkdir NginxStatus"
  - Entrar no diretório "cd NginxStatus"
  - Criar o script "touch status.sh"
  - Editar o script "vi status.sh"

> Resumo dos comandos:
```sh
$> su root
#> cd /
#> mkdir NginxStatus
#> cd NginxStatus
#> touch status.sh
#> vi status.sh
```
- `su root`: loga como usuario root
- `cd`: significa change directory/mudar diretorio.
- `cd /`: entra no diretorio raiz
- `mkdir NginxStatus`: cria o diretorio/pasta NginxStatus, onde colocaremos nossos scripts.
- `cd NginxStatus`: entra no diretorio NginxStatus.
- `touch status.sh`: o touch cria um arquivo vazio, então ele está criando o arquivo status.sh
- `vi status.sh`: o vi é o editor, então ele vai abrir o arquivo status.sh com o editor de texto vi.

> O editor vi funciona de modo geral em dois modos/estados: o estado de inserção e o estado de não inserção, quando você clicar em **ESC** ele entra no modo de não inserção, que é o modo emque você pode inserir comandos:

> alguns comandos mais usados (Esc + comando):
> 
    - -i (inserir, ele abre o modo de inserção onde cursor está no momentos.)
    -  -A (inserir, ele abre o modo de inserção no final da linha em que o cursor se encontra)
    -  -dd (Deleta a linha em que o cursor se encontra)
    - -x (Deleta a letra que está na frente do cursor)
    - :w (salva)
    - :q! (força a saída)
    -  :wq (salva e sai)
>
> **Detalhe**: para se mover sem problemas, clique em ESC e use as teclas de seta pra cima, baixo, etc. Estarei deixando um vídeo para auxiliar no uso do vi, na seção das [referências](https://github.com/Yyfii/NginxServer/blob/main/README.md#referencias)


  ``` sh
  #!/bin/bash 
  
  if [ $(systemctl | grep -i nginx | wc -l) -gt 0 ]
  then
          echo "$(date): Nginx - Está rodando - ONLINE" >> /NginxStatus/status.log
  else
          echo "$(date): Nginx - Não está rodando - OFFLINE" >> /NginxStatus/status.log
  fi
  ``` 
`descrição`:
- `#!/bin/bash` **todo script shell deve iniciar com essa linha, pois indicar o shell que estará executando esse script, no caso o Bash **
- Fica extra atento até mesmo para os espaços! o shell script é muito sensível a isso! *principalmento nos antes e depois dos colchetes no if.
  > Vamos lá! No código estamos utilizando uma estrutura condicional if else fi:
  
  - `if `: inicia a condição,  e devem ser passados PARÂMETROS.
  -   `[ $(systemctl | grep -i nginx | wc -l) -gt 0 ]` :
     - `[ conditions ]`" : local onde devem ser colocados os parâmetros/condições, sempre com um espaço antes e depois do colchete.
     - `$(comands)` : também pode ser subistituído por \`comands`, é usado para colocar comandos, como o ls. Exemplo: $(ls).
     - `systemctl` : Gerencia todos os serviços ativos e inativos.
     - ` | `: chamado de pipe, ele roteia o resultado do comando anterior a ele, no caso o systemctl , para o próximo comando.
     - ` grep -i nginx ` : o grep faz uma busca em todas as ocorrencias da palavra nginx, e o -i é usado pois o linux é case sensitive, ao usá-lo ele não vai se importar caso tenham ocorrencias da palavra com letras maiúsculas ou minúsculas. Em síntese ele ele vai buscar no resultado do systemctl(busca todos os serviços ativos) se o nginx aparece(se ele está ativo).
     - ` wc -l ` : conta as ocorrências da palavra nginx.
     - ` -gt 0 ` : pega o resultado dos comandos( a quantidade de ocorrencia) e pergunta se é gt(greater than/maior que) 0.
     - ` then ` : caso a condição seja satisfeita, ou seja, as ocorrências existirem, em outras palavras, o nginx estiver ativo.
     - ` echo _expressao_ ` : é como um print, ele vai chamar algum comando, palavra, string, variável, etc.
     - ` $(date) ` : como citado anteriormente o `$()` é usado para executar comandos, e neste caso ele está executando o comando date, que traz a hora e data.
     - ` : ` : é apenas o caractere dois pontos, apenas para melhorar o visual.
     - ` Nginx - Está rodando - ONLINE ` : o texto a ser exibido.
     - ` >> ` : esse símbolo pega a expressao do echo e redireciona para algum lugar, como por exemplo um arquivo.
     - ` /NginxStatus/status.log ` : o arquivo de log, onde as atualizações a cada 5 min serão enviadas.

> Agora para salvar **ESC + :wq**
> Para ver se ele está funcionando:
```
#> ls 
#> cd /
#> chmod +x /NginxStatus/status.sh
#> chmod +x /NginxStatus/status.log
#> /NginxStatus/status.sh
#> ./NginxStatus/status.log
```
- `ls`: lista todos os arquivos e diretorios do caminho atual (menos os ocultos, p/isso use ls -a).
- `cd /`: cd(change directory/mudar diretorio) ele muda o diretório para o /, que é o diretorio raiz onde o /home, /etc e outros diretorios principais ficam.
- `chmod +x /NginxStatus/status.sh`: vamos por partes, o `chmod` muda as permissões de acesso para arquivos ou diretórios, o `+x` faz com que o usuario dono do arquivo ou diretorio(ou seja quem o criou), o grupo do dono do aquivo e outros que não são o usuario dono ou não fazem parte do grupo do usuario dono posssam executar o arquivo, o `+` adiciona a permissão a todos e o `x` significa executar, em síntese, todos podem executar o arquivo, que arquivo? o `/NginxStatus/status.sh`.
- `./NginxStatus/status.sh` : o `./`, esse ponto antes do barra significa a ação de executar um script, então essa linha vai executar o status.sh.
  
> Você verá que no status.sh não retorna nada, por que ele está redirecionado para o status.log, que traz um resultado.
> Por aqui já está bom, faremos umas modificações mais adiante, mas por agora seguiremos para outra etapa.

# Step 7: Automatização do script shell com o cron.
> O cron no linux provê um serviço de agendamento de tarefas para usuários do sistema, é um processo simples.

obs: se estiver como root não precisa do _-u ana_ detalhe ( ana é o nome do usuario)
```
#> cd /
#> crontab -u ana -e
```
>Ele vai perguntar qual editor você quer usar, neste tutorial estamos usando o vi/vim, mas fica a sua preferencia. Para mudar o editor após ter escolhido, use o comando `select-editor`,após escolher ele vai redirecionar para um arquivo, saia dele com o **:q!**.
>
>Antes de prosserguirmos, tenha certeza de lembrar o caminho do seu arquivo status.sh. Para ter certeza do seu caminho, vá até o diretório raiz `cd / ` e tente acessar o arquivo com esse mesmo  que você colocou, no caso da minha máquina `cat /NginxStatus/status.sh`, se funcionar está correto, se não funcionar, não precisa se apavorar, na pasta em que você estiver dê um `ls`, e se a pasta NginxStatus aparecer, dê um `pwd`, em seguida `cd NginxStatus/`, dê outro `pwd`, dê  um `ls`, se o seu status.sh aparecer então o seu caminho correto é o resultado o `pwd/status.sh -> resultado/status.sh`.
>
```
#> crontab -e

```

![cron vazio](https://github.com/user-attachments/assets/97707d9c-ccc2-495b-9f43-6153067e3150)

> Agora iremos adicionar uma linha, depois de todos esses comentário. **precisa de uma linha vazia após o comando,pois senão ele não roda**
``` sh
*/1 * * * * /NginxStatus/status.sh >> /NginxStatus/status.log

```
>![cron after edit](https://github.com/user-attachments/assets/94c8640e-4119-444e-a02a-f8603774fd80)

> Vamos entender o que a linha significa! caso queia mais detalhes, deixei um link nas :point_right: [referencias](https://www.certificacaolinux.com.br/comando-linux-cron/) sobre o cron.

> Cada asterisco ( * * * * * ) -> (minutos horas diasDoMes Mes diaSemana) e devem ser separados por um espaço; o "( */1 * * * *) indica que a cada minuto ele fará alguma coisa, poderia ser um echo " running" por exemplo, mas no nosso caso ele executará um script a cada minuto(depois ajustaremos para cinco minutos como na proposta).

> `/NginxStatus/status.sh ` é o script que será executado a cada minuto.

> ` >> /NginxStatus/status.log` indica que o resultado da execução do script será concatenado/adicionado ao arquivo status.log.

> Então a cada minuto ele executará o script status.sh (que está verificando se o nginx está ativo ou não), e quando ele receber o resultado ele vai salvar o resultado em uma linha no arquivo status.log.

> Para saber se isso está funcionando:
```
#> cd /
#> cd NginxStatus/
#> ./status.sh
#> ./status.log
ou
#> tail -f status.log
```
`tail -f status.log`: tail pega as últimas linhas de um arquivo e o -f indica para ele pegar em tempo real, então a cada execução do status.sh ele adiciona uma linha sobre o status do nginx no final do status.log, o tail vai pegando essas linhas em tempo real, então se você deixar esses comando rodando, vai conseguir ver que o processo está realmente funcionando.

> ![status.log](https://github.com/user-attachments/assets/5982d1d0-23e2-43d1-8498-28882b5876e1)

> Para parar o tail *Ctrl + z*

# Step 8: Redirecionando para duas saídas (situações 1 - online 2 - offline)

> Agora iremos criar dois arquivos, e ao invés dele criar um echo pra cada situação, ele vai executar um script para cada situação. E caso queira relembrar algum comando, visite a nossa [seção de descrição dos comandos](https://github.com/Yyfii/NginxServer/blob/main/README.md#descri%C3%A7%C3%A3o-dos-principais-comandos)

```
#> cd /
#> cd NginxStatus/
#> touch online.sh offline.sh
#> chmod +x online.sh
#> chmod +x offline.sh
#> export PATH=$PATH:/NginxStatus/:
#> vi status.sh

```
`export PATH=$PATH:/NginxStatus/:` : está adicionando o camiondo da pasta que criamos nossos scripts na variavel de caminhos globais PATH, assim poderemos acessar os scripts sem problema.

>Modifique as linhas para:
```
 #!/bin/bash
if [ $(systemctl | grep -i nginx | wc -l) -gt 0 ]
then
        /NginxStatus/online.sh

else
        /NginxStatus/offline.sh
fi
~  
```
> Assim, ao invés dele fazer um echo dizendo que o servidor está ativo, ele vai redirecionar para um outro script `online.sh` e nesse script terá uma mensagem de ativo que será enviada para o status.log, e enfim ao usarmos o tail -f status.log, poderemos ver se o nginx está ativo ou não.
`/NginxStatus/online.sh`: antes estavamos utilizando o `.` para executar os scripts, mas como adicionamos o caminho na PATH, não precisamos mais fazer isso.

> ![nova versao status.log](https://github.com/user-attachments/assets/5d98e528-99bc-4c49-884a-741f9441e369)

> Agora editaremos o arquivo `online.sh` com o vi.
```
#> cd /
#> cd NginxStatus/
#> vi online.sh
```
> e digite o seguinte código, com atenção aos espaços(não devem haver espaços antes e depois do espaço em `msg=`).
```
#!/bin/bash

msg="Data: $(date) \nServiço: Nginx \nONLINE -  Rodando perfeitamente, fique tranquile (o..o) $(whoami)"
echo -e "$msg" >> /NginxStatus/status.log
```
> ![online](https://github.com/user-attachments/assets/a34a94bd-c203-4af8-b5f8-77f4ac9708f0)

> Salve o arquivo `:wq` e agora entre no arquivo offline.sh ou você pode simplesmente fazer uma cópia do online.sh e colocar no offline.sh
```
cp online.sh offline.sh
```
> Agora entre no arquivo e faça as mudanças necessárias(apenas a mensagem), você pode modificar da forma que deseja.
```
#> vi offline.sh
```
> e digite:
```
#!/bin/bash

msg="Data: $(date) \nServiço: Nginx \nOFFLINE -  Sono Profundo, ( - . -)  $(whoami)"
echo -e "$msg" >> /NginxStatus/status.log
```
> ![offline](https://github.com/user-attachments/assets/75ca6286-8b55-4008-968f-7f6d75de0ed7)

> E agora, entre no crontab:
```
#> cd /
#> crontab -e
```
> E edite a linha para:
```
*/1 * * * * /NginxStatus/status.sh >> /NginxStatus/status.log 2>&1
```
> Salve e saia `:wq`

> ![crontab last version](https://github.com/user-attachments/assets/5f896d0f-f11e-49ee-8bf4-c29c1d6592fc)

> Agora verfificando se está rodando perfeitamente:
```
#> sudo systemctl restart nginx
#> tail -f status.log
```
>  ![verificacao](https://github.com/user-attachments/assets/3e16548b-091d-40c5-86d1-8a54a05bd0b3)

> Se você quiser usar esses comandos em qualquer lugar do sistema:
> 
 ```
#> echo $PATH
#> export PATH=$PATH:/NginxStatus/:
#>  echo $PATH
#>  status.sh
#>  status.log
``` 
> última mudança depois de checar se tudo está ok, mudando o tempo para 5 minutos:
```
*/5 * * * * /NginxStatus/status.sh >> /NginxStatus/status.log 2>&1
```
> ![mudando5](https://github.com/user-attachments/assets/c7bde43f-fc13-4c8b-b382-591620c62279)

> Verficando e parando o nginx:
```
#> sudo systemctl stop nginx
#> tail -f NginxStatus/status.log
```
> ![parando nginx](https://github.com/user-attachments/assets/3149ee9a-9080-49e4-a9c1-e63ffb044077)

  # Step 9: Colocando o versão do script na mensagem.

  > Vamos adicionar ao nosso script status.sh um código que gere a versão do script atual, e a cada vez que o script é executado, uma nova versão é criada. Exemplo: versão 1, 2 etc.
> Então, primeiramente vamos abrir o arquivo status.sh
```
vi status.sh
```
> Depois do #!/bin/bash vamos adicionar o seguinte código:
```
#!/bin/bash

VERSION_FILE="/NginxStatus/version.txt"

if [ ! -f "$VERSION_FILE" ]; then
            echo "1" > "$VERSION_FILE"
fi

VERSION=$(cat "$VERSION_FILE")

NEW_VERSION=$((VERSION + 1))

echo "$NEW_VERSION" > "$VERSION_FILE"

echo "Versão atual do script: $VERSION"
```
`VERSION_FILE="/NginxStatus/version.txt"` : Cria uma variável "VERSION_FILE" que vai armazenar o caminho do arquivo de versão, que será criado nas próximas linhas. Nesse arquivo colocaremos o número da versão do arquivo, esse número é incrementado a cada execução.

```
if [ ! -f "$VERSION_FILE" ]; then
            echo "1" > "$VERSION_FILE"
fi
```
- O código acima verifica se o arquivo contido na variável VERSION_FILE existe.
- `!` : nega algo.
- `-f`: Retorna se um arquivo existe ou não (true ou false)
- `-f "$VERSION_FILE"`: Retorna se o arquivo version.txt existe ou não.
- Então, se `[! -f "$VERSION_FILE" ]` o arquivo version.txt não existir, ele:
- ` echo "1" > "$VERSION_FILE"` : coloca o valor ** 1 ** no version file, pois se ele não existir quer dizer que não foi criado e portanto, que ainda não foi executado, nesse caso essa seria a primeira versão dele.

- `VERSION=$(cat "$VERSION_FILE")`: Agora ele cria uma variável chamada VERSION, e dentro dela ele armazena o resultado da execução de um comando, o cat $VERSION_FILE, que é a variável que está armazenando o arquivo que guarda o número da versão, então o que ele realmente está colocando como valor na variável VERSION é o número da versão (se for a primeira vez executando o script, seria o 1).
- `NEW_VERSION=$((VERSION + 1))`: Aqui, ele está fazendo o incremento, ele cria uma variável NEW_VERSION que armazena o resultado da execução de um comando,`$((VERSION + 1))`, que pega o valor da versão atual e soma 1, pois toda vez que o script é executado, o número da versão muda.
- `echo "$NEW_VERSION" > "$VERSION_FILE"`: Nesse trecho de código, estamos chamando o valor da NEW_VERSION(já incrementado, o valor da nova versão) e estamos substituindo o valor da versão no arquivo version.txt para o valor da nova versão.
- `echo "Versão atual do script: $VERSION`: E aqui, exibimos o novo valor.

> O código completo ficaria da seguinte forma: 
```
#!/bin/bash

VERSION_FILE="/NginxStatus/version.txt"

if [ ! -f "$VERSION_FILE" ]; then
            echo "1" > "$VERSION_FILE"
fi

VERSION=$(cat "$VERSION_FILE")

NEW_VERSION=$((VERSION + 1))

echo "$NEW_VERSION" > "$VERSION_FILE"

echo "Versão atual do script: $VERSION"

if [ $(systemctl | grep -i nginx | wc -l) -gt 0 ]

then
        /NginxStatus/online.sh

else
        /NginxStatus/offline.sh
fi
```
![novo status](https://github.com/user-attachments/assets/977349b3-b75e-4030-8b34-451c8ad1b442)

> Agora iremos editar o arquivo online.sh, para que a versão seja mostrada na mensagem de status:

- Iremos adicionar após o ` #!/bin/bash`: `VERSION=$(cat /NginxStatus/version.txt)`
- E após o $(date) na variável msg, `\nVersao: $VERSION`

O código ficará da seguinte forma:
```
#!/bin/bash

VERSION=$(cat /NginxStatus/version.txt)

msg="Data: $(date) \nVersao: $VERSION \nServiço: Nginx \nONLINE -  Rodando perfeitamente, fique tranquile (o..o) $(whoami)"
echo -e "$msg" >> /NginxStatus/status.log
```
![online](https://github.com/user-attachments/assets/12c97344-91b9-47ee-8ad1-3a83d2b55591)
> Por fim devemos editar o offline.sh, faça o mesmo processo:

- Iremos adicionar após o ` #!/bin/bash`: `VERSION=$(cat /NginxStatus/version.txt)`
- E após o $(date) na variável msg, `\nVersao: $VERSION`

O código ficará da seguinte forma:
```
#!/bin/bash

VERSION=$(cat /NginxStatus/version.txt)

msg="Data: $(date) \nVersao: $VERSION \nServiço: Nginx \nOFFLINE -  Sono Profundo, ( - . -)  $(whoami)"
echo -e "$msg" >> /NginxStatus/status.log
```
![offline](https://github.com/user-attachments/assets/b062ee2e-e9b1-4d68-ae9b-837499ac4b74)


Agora reiniciaremos o ssh para que as informações sejam atualizadas:

```
systemctl restart ssh
```

Espere o tempo definido anteriormente, e teste o status.log: 

```
tail -f status.log
```
O resultado será semelhante a imagem abaixo:

![Resultado](https://github.com/user-attachments/assets/725bb2ea-6a8e-47c3-b2e7-28a907373cde)

***
Se pararmos o nginx:

```
systemctl stop ssh
```
![stopped](https://github.com/user-attachments/assets/aa2a62ed-7eaa-4e1e-868f-201dc19669c5)

Como você pode perceber, ele agora está mostrando as notificações do nginx parado. Então está funcionando perfeitamente! :relaxed:

Bom trabalho até aqui! :shipit:

Se por acaso algum desses passos não funcionou para você, estou deixando todos os links que utilizei e que você pode utilizar seja para instalar o Ubuntu até os comandos utilizados!

## DESCRIÇÃO DOS PRINCIPAIS COMANDOS

- `su root`: loga como usuario root.

- `cd`: significa change directory/mudar diretorio.

- `touch arquivo`: o touch cria um arquivo vazio, então ele vai criar o arquivo.

- `vi arquivo`: o vi é o editor, então ele vai abrir o arquivo com o editor de texto vi.

- `tail -f arquivo`: tail pega as últimas linhas de um arquivo e o -f indica para ele pegar em tempo real.

- ` >> ` : concatena, redireciona or resultado de um comando por exemplo: pwd >> file.tx, ele pega o resultado do comando e concatena ao que já se encontra no arquivo file.txt, se fosse pwd > file.tx, ele não concatenaria, mas substituiria o conteudo pela saida do comando pwd.

- `chmod +x arquivo`: vamos por partes, o `chmod` muda as permissões de acesso para arquivos ou diretórios, o `+x` faz com que o usuario dono do arquivo ou diretorio(ou seja quem o criou), o grupo do dono do aquivo e outros que não são o usuario dono ou não fazem parte do grupo do usuario dono posssam executar o arquivo, o `+` adiciona a permissão a todos e o `x` significa executar, em síntese, todos podem executar o arquivo ou diretorio.
  
- `./script.sh` : o `./`, esse ponto antes do barra significa a ação de executar um script, então essa linha vai executar o script.sh.
  
- `if `: inicia a condição,  e devem ser passados PARÂMETROS.
- `[ $(systemctl | grep -i nginx | wc -l) -gt 0 ]` :
- `[ conditions ]`" : local onde devem ser colocados os parâmetros/condições, sempre com um espaço antes e depois do colchete.
- `$(comands)` : também pode ser subistituído por \`comands`, é usado para colocar comandos, como o ls. Exemplo: $(ls).
- `systemctl` : Gerencia todos os serviços ativos e inativos.
- ` | `: chamado de pipe, ele roteia o resultado do comando anterior a ele, no caso o systemctl , para o próximo comando.
- ` grep -i nginx ` : o grep faz uma busca em todas as ocorrencias da palavra nginx, e o -i é usado pois o linux é case sensitive, ao usá-lo ele não vai se importar caso tenham ocorrencias da palavra com letras maiúsculas ou minúsculas. Em síntese ele ele vai buscar no resultado do systemctl(busca todos os serviços ativos) se o nginx aparece(se ele está ativo).
- ` wc -l ` : conta as ocorrências da palavra nginx.
- ` -gt 0 ` : pega o resultado dos comandos( a quantidade de ocorrencia) e pergunta se é gt(greater than/maior que) 0.
- ` then ` : caso a condição seja satisfeita, ou seja, as ocorrências existirem, em outras palavras, o nginx estiver ativo.
- ` echo _expressao_ ` : é como um print, ele vai chamar algum comando, palavra, string, variável, etc.
- ` $(date) ` : como citado anteriormente o `$()` é usado para executar comandos, e neste caso ele está executando o comando date, que traz a hora e data.
- ` : ` : é apenas o caractere dois pontos, apenas para melhorar o visual.
- ` Nginx - Está rodando - ONLINE ` : o texto a ser exibido.
- ` >> ` : esse símbolo pega a expressao do echo e redireciona para algum lugar, como por exemplo um arquivo.
- ` /NginxStatus/status.log ` : o arquivo de log, onde as atualizações a cada 5 min serão enviadas.
- `!` : nega algo.
- `-f`: Retorna se um arquivo existe ou não (true ou false)
- `-f "$VERSION_FILE"`: Retorna se o arquivo version.txt existe ou não.
- Então, se `[! -f "$VERSION_FILE" ]` o arquivo version.txt não existir, ele:
- ` echo "1" > "$VERSION_FILE"` : coloca o valor ** 1 ** no version file, pois se ele não existir quer dizer que não foi criado e portanto, que ainda não foi executado, nesse caso essa seria a primeira versão dele.
- `VERSION=$(cat "$VERSION_FILE")`: Agora ele cria uma variável chamada VERSION, e dentro dela ele armazena o resultado da execução de um comando, o cat $VERSION_FILE, que é a variável que está armazenando o arquivo que guarda o número da versão, então o que ele realmente está colocando como valor na variável VERSION é o número da versão (se for a primeira vez executando o script, seria o 1).
- `NEW_VERSION=$((VERSION + 1))`: Aqui, ele está fazendo o incremento, ele cria uma variável NEW_VERSION que armazena o resultado da execução de um comando,`$((VERSION + 1))`, que pega o valor da versão atual e soma 1, pois toda vez que o script é executado, o número da versão muda.
- `echo "$NEW_VERSION" > "$VERSION_FILE"`: Nesse trecho de código, estamos chamando o valor da NEW_VERSION(já incrementado, o valor da nova versão) e estamos substituindo o valor da versão no arquivo version.txt para o valor da nova versão.
**
# REFERENCIAS
:pushpin: [Aula de VIM ou VI - Aprenda TUDO em 10 minutos! | LPIC-1](https://www.youtube.com/watch?v=Ep_uf_q3ST0)

:pushpin: [Usando vi no Linux](https://receitasdecodigo.com.br/ubuntu/usando-vi-no-linux)

:pushpin: [Comando cron no Linux (agendamento de tarefas) [Guia Básico]](https://www.certificacaolinux.com.br/comando-linux-cron/)

:pushpin: [UFW is blocking all even when I set rules to allow](https://askubuntu.com/questions/541675/ufw-is-blocking-all-even-when-i-set-rules-to-allow)

:pushpin: [Top 3 ways to check if a service is enabled in Linux](https://www.howtouselinux.com/post/check-if-a-service-is-enabled-in-linux)

:pushpin: [Specify an Editor for Crontab](https://www.baeldung.com/linux/crontab-editor)


:pushpin: [How to view status of a service on Linux using systemctl](https://www.cyberciti.biz/faq/systemd-systemctl-view-status-of-a-service-on-linux/)

:pushpin: [Complete list of github markdown emoji markup](https://gist.github.com/rxaviers/7360908)

:pushpin: [WSL 2 - Instalando Ubuntu 20.04 no Windows 10 com Windows Subsystem for Linux
](https://www.youtube.com/watch?v=_Wp2nWtTBBY)

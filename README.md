# NginxServer
<p> Colocando um servidor Nginx no ar, utilizando a WSL2 e Ubuntu 20.04 no Windows Subsystem for Linux e criação de script de status do serviço Nginx.</p>

# DOCUMENTAÇÃO
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
     - Clique em ativar e desativar recursos do windows.
     - Irá aparecer uma lista, procure "Subsistema do Windows para Linux".
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
     - 
     ![Painel Inicial](https://github.com/user-attachments/assets/81118493-3b19-473c-b864-7b281a339c79)

     - Clique nele.
     - Ao carregar ele irá pedir um nome de usuário.
     - E também pedirá uma senha. (*Tenha certeza de anotá-la em algum lugar seguro para não esquecê-la)
  # Step 3: Instalando o Nginx.
     - Antes de instalarmos qualquer coisa, iremos atualizar os pacotes.
     - Digite "sudo apt update && sudo apt upgrade".
     - Se ele fizer alguma pergunta digite "y" para confirmar a instalação dos pacotes.
     - Agora digite "sudo apt install nginx"
     - Se ele fizer alguma pergunta digite "y" para confirmar a instalação dos pacotes.
  # Step 4: Configurando o Firewall.
     - Antes de testar, precisamos ter certeza de que o firewall não irá tentar impedir a ativação do nginx.
     - Listando as configurações das aplicações: "sudo ufw app list"
       - Nginx Full ( Abre a porta 80 (tráfico web não criptografado) e 443 (TLS/SSL tráfico criptografado)
       - Nginx HTTP ( Abre apenas a porta 80)
       - Nginx HTTPS (Abre apenas a porta 443)
    - Por agora iremos ativar apenas o tráfico na porta 80, ou seja o Nginx HTTTP
    - Digite "sudo ufw allow 'Nginx HTTP'
    - Digite "sudo ufw disable" e em seguida "sudo ufw enable" para reiniciar o firewall.
    - Verfique o status: "sudo ufw status"
  # Step 5: Verificando o Nginx.
    - Podemos checar se ele está rodando com o comando "systemctl status nginx"
    - - Comandos usados no nginx:
      - sudo systemctl stop nginx (para o serviço)
      - sudo systemctl systemctl start nginx (inicia o serviço)
      - sudo systemctl restart nginx (reiniciar o serviço)
      - sudo systemctl reload nginx (recarregar  o serviço sem parar as conexões)
      - sudo systemctl disable nginx (desabilitar o serviço)
      - sudo systemctl enable nginx (habilitar o serviço)
  # Step 6: Criando o script de validação
  
  > Iremos utilizar um shell script(irei chamá-lo de "status.sh"), com uma extensão .sh, esse script validará se o serviço está ativo ou não e enviaá o seu status para um diretório que iremos criar(irei chamá-lo de "StatusNginx").
  > O status.sh conterá Data e hora, nome do serviço,  status, uma mensagem personalizada indicando seu status( online ou offline).
  O status.sh gerará dois arquivos de saída:
  - online.sh - arquivo de serviço online.
  - offline.sh - arquivo de serviço offline.
  Esse script será executado a cada 5 minutos, e a cada execução ele terá uma nova versão.(Ex: statusv1.sh)
  
  `História do script`: Se o nginx aparecer quando pesquisarmos todos os serviços ativos do sistema -> ele está rodando, e iremos executar o script online.sh e mandar a execfução para o arquivo de log para que possa servir como um histórico de todas as execuções. Se ele não aparecer, será executado o arquivo offline.sh e mandado a ocorrencia para o arquivo de log, o status.log.
  
  Primeiramente iremos criar um diretorio:
  - Logue como root "su root"
  - Entre no diretorio raiz "cd / "
  - Criar o diretório "mkdir NginxStatus"
  - Entrar no diretório "cd NginxStatus"
  - Criar o script "touch status.sh"
  - Editar o script "vi status.sh"

    > O editor vi funciona de modo geral em dois modos/estados: o estado de inserção e o estado de não inserção, quando você clicar em **ESC** ele entra no modo de não inserção, que é o modo emque vocÊ pode inserir comandos:
    > alguns comandos mais usados (Esc + comando):
    > - -i (inserir, ele abre o modo de inserção onde cursor está no momentos.)
    > - -A (inserir, ele abre o modo de inserção no final da linha em que o cursor se encontra)
    > - -dd (Deleta a linha em que o cursor se encontra)
    > - -x (Deleta a letra que está na frente do cursor)
    > - :w (salva)
    > - :q! (força a saída)
    > - :wq (salva e sai)
    > **Detalhe**: para se mover sem problemas, clique em ESC e use as teclas de seta pra cima, baixo, etc.

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
- #!/bin/bash **todo script shell deve iniciar com essa linha, pois indicar o shell que estará executando esse script, no caso o Bash **
- Fica extra atento até mesmo para os espaços! o shell script é muito sensível a isso! *principalmento nos antes e depois dos colchetes no if.
- Vamos lá! No código estamos utilizando uma estrutura condicional if else fi:
  -  if : inicia a condição,  e devem ser passados PARÂMETROS.
  -   [ $(systemctl | grep -i nginx | wc -l) -gt 0 ] :
     - "[ conditions ]" : local onde devem ser colocados os parâmetros/condições, sempre com um espaço antes e depois do colchete.
     - "$(comands)" : também pode ser subistituído por \`comands`, é usado para colocar comandos, como o ls. Exemplo: $(ls).
     - "systemctl" : Gerencia todos os serviços ativos e inativos.
     - " | ": chamado de pipe, ele roteia o resultado do comando anterior a ele, no caso o systemctl , para o próximo comando.
     - " grep -i nginx " : o grep faz uma busca em todas as ocorrencias da palavra nginx, e o -i é usado pois o linux é case sensitive, ao usá-lo ele não vai se importar caso tenham ocorrencias da palavra com letras maiúsculas ou minúsculas. Em síntese ele ele vai buscar no resultado do systemctl(busca todos os serviços ativos) se o nginx aparece(se ele está ativo).
     - " wc -l " : conta as ocorrências da palavra nginx.
     - " -gt 0 " : pega o resultado dos comandos( a quantidade de ocorrencia) e pergunta se é gt(greater than/maior que) 0.
     - " then " : caso a condição seja satisfeita, ou seja, as ocorrências existirem, em outras palavras, o nginx estiver ativo.
     - " echo _expressao_ " : é como um print, ele vai chamar algum comando, palavra, string, variável, etc.
     - " $(date) " : como citado anteriormente o "$()" é usado para executar comandos, e neste caso ele está executando o comando date, que traz a hora e data.
     - " : " : é apenas o caractere dois pontos, apenas para melhorar o visual.
     - " Nginx - Está rodando - ONLINE " : o texto.
     - " >> " : esse símbolo pega a expressao do echo e redireciona para algum lugar, como por exemplo um arquivo.
     - " /NginxStatus/status.log " : o arquivo de log, onde as atualizações a cada 5 min serão enviadas.

> Agora para salvar **ESC + :wq**
> Para ver se ele está funcionando:
```
#> ls 
#> cd /
#> chmod +x /NginxStatus/status.sh
#> chmod +x /NginxStatus/status.log
#> ./NginxStatus/status.sh
#> ./NginxStatus/status.log
```
> Você verá que no status.sh não retorna nada, por que ele está redirecionado para o status.log, que traz um resultado.
> Por aqui já está bom, faremos umas modificações mais adiante, mas por agora seguiremos para outra etapa.

# Step 7: Automatização do script shell com o cron.
> O cron no linux provê um serviço de agendamento de tarefas para usuários do sistema, é um processo simples.

obs: se estiver como root não precisa do _-u ana_ detalhe, ana é o nome do usuario)
```
#> cd /
#> crontab -u ana -e
```
>Ele vai perguntar qual editor você quer usar, neste tutorial estamos usando o vi/vim, mas fica a sua preferencia. Para mudar o editor após ter escolhido, use o comando `select-editor`,após escolher ele vai redirecionar para um arquivo, saia dele com o **:q!**.
>
>Antes de prosserguirmos, tenha certeza de lembrar o caminho do seu arquivo status.sh. Para ter certeza do seu caminho, vá até o diretório raiz "cd / " e tente acessar o arquivo com esse mesmo  que você colocou, no caso da minha máquina "cat /NginxStatus/status.sh", se funcionar está correto, se não funcionar, não precisa se apavorar, na pasta em que você estiver dê um ls, e se a pasta NginxStatus aparecer, dê um "pwd", em seguida "cd NginxStatus/", dê outro "pwd", dê  um "ls", se o seu status.sh aparecer então o seu caminho correto é o resultado o"pwd"/status.sh -> resultado/status.sh.
>
```
#> crontab -e

```

![cron vazio](https://github.com/user-attachments/assets/97707d9c-ccc2-495b-9f43-6153067e3150)

> Agora iremos adicionar uma linha, depois de todos esses comentário. **precisa de uma linha vazia após o comando,pois senão ele não roda"
``` sh
*/1 * * * * /NginxStatus/status.sh >> /NginxStatus/status.log

```
>![cron after edit](https://github.com/user-attachments/assets/94c8640e-4119-444e-a02a-f8603774fd80)

> Vamos entender o que a linha significa!

> Cada asterisco ( * * * * * ) -> (minutos horas diasDoMes Mes diaSemana) e devem ser separados por um espaço; o "( */1 * * * *) indica que a cada minuto ele fará alguma coisa, poderia ser um echo " running" por exemplo, mas nop nosso caso ele executará um script a cada minuto(depois ajustaremos para cinco minutos como na proposta).

> "/NginxStatus/status.sh " é o script que será executado a cada minuto.

> ">> /NginxStatus/status.log" indica que o resultado da execução do script será concatenado/adicionado ao arquivo status.log.

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
> ![status.log](https://github.com/user-attachments/assets/5982d1d0-23e2-43d1-8498-28882b5876e1)

> Para parar o tail *Ctrl + z*

# Step 8: Redirecionando para duas saída (situações 1 - online 2 - offline)

> Agora iremos criar dois arquivos, e ao invés dele criar um echo pra cada situação, ele vai executar um script para cada situação.

```
#> cd /
#> cd NginxStatus/
#> touch online.sh offline.sh
#> chmod +x online.sh
#> chmod +x offline.sh
#> export PATH=$PATH:/NginxStatus/:
#> vi status.sh

```
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
> ![nova versao status.log](https://github.com/user-attachments/assets/5d98e528-99bc-4c49-884a-741f9441e369)
>
```
#> cd /
#> cd NginxStatus/
#> vi online.sh
```
> ![online](https://github.com/user-attachments/assets/a34a94bd-c203-4af8-b5f8-77f4ac9708f0)

> Salve e:
```
#> vi offline.sh
```
> ![offline](https://github.com/user-attachments/assets/75ca6286-8b55-4008-968f-7f6d75de0ed7)

> E agora o crontab:
```
#> cd /
#> crontab -e
```
> ![crontab last version](https://github.com/user-attachments/assets/5f896d0f-f11e-49ee-8bf4-c29c1d6592fc)

> Agora verfificando se está rodando perfeitamente:
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
> ![mudando5](https://github.com/user-attachments/assets/c7bde43f-fc13-4c8b-b382-591620c62279)

> Verficando e parando o nginx:
> ![parando nginx](https://github.com/user-attachments/assets/3149ee9a-9080-49e4-a9c1-e63ffb044077)


# REFERENCIAS
https://askubuntu.com/questions/541675/ufw-is-blocking-all-even-when-i-set-rules-to-allow
https://www.howtouselinux.com/post/check-if-a-service-is-enabled-in-linux
https://www.baeldung.com/linux/crontab-editor
https://www.certificacaolinux.com.br/comando-linux-cron/
https://www.cyberciti.biz/faq/systemd-systemctl-view-status-of-a-service-on-linux/
https://gist.github.com/rxaviers/7360908
https://github.com/gui-lirasilva/Edige-POO/blob/master/README.md
https://dev.to/sabrinabarros/o-que-e-markdown-e-como-ele-pode-melhorar-o-seu-readme-no-github-2n2l
https://www.alura.com.br/artigos/escrever-bom-readme

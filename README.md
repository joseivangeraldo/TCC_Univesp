# Arquivos usados no TCC  da UNIVESP

Recursos necessários:

- [Jupyter Notebook](https://jupyter.org/)
- [Python 3](
- [Pandas](#link)
- [Selenium](#link)

## <a id="instalacao">🔨 Instalação do Ambiente</a>
 Instalação Imagem Docker, para programar páginas diretamente em um servidor Web.
Obs: Estes exemplos foram formulados totalmente em um ambiente computacional dentro da nuvem. Particulamente no codespace do Github e Gitpod. Na atualidade são as melhores plataformas para isto, balanceando os custos e beneficios. Se você não conhece ou não sabe como acessa-los, [clique aqui.](https://docs.github.com/en/codespaces/developing-in-codespaces/opening-an-existing-codespace/)

No codespace abra um terminal ou teclas use <kbd>Ctrl</kbd> + <kbd>'</kbd> 
Vamos ver antes se existem imagens criadas, só para verificar
```
$ docker images
```
Se não tiver nenhuma imagem docker será mostrado uma tabela vazia:
```
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```

Após crie a pasta de trabalho:
```
$ mkdir ServerApache
```
O nome do diretório pode ser este ou qualquer outro de sua preferência.

Depois entre dentro deste diretório:
```
$ cd ServerApache
```
Criar uma página chamada index.html básica dentro deste diretório, para verificar que ocorreu uma instalação perfeita.
Segue um modelo básico abaixo:
```
<html>
    <head> </head>
    <body>
        <H1> 
            A Pagina Inicial Funcionou
        </H1>
    </body>
</html>
```

Crie um arquivo Dockerfile. O Codespace GitHub e Gitpod já vem com Visual Studio configurado.
Tem de ter este nome 'Dockerfile' sem nenhuma extensão, assim digite:
```
$ code Dockerfile
```

Este Dockerfile, que orquestrará todas as dependencias do ambiente para a rodar a imagem Docker.
Seque a sintaxe do modelo:
```
 FROM httpd:2.4  
 COPY ./ /usr/local/apache2/htdocs/ 
 RUN ["apt-get", "update"]  
 RUN ["apt-get", "install", "-y", "vim"]
 ```
 Em COPY , copiamos tudo que esta na pasta e coloca na pasta aonde são publicada as paginas web do container.
 Nas duas ultimas linhas estamos atualizando a distribuição Linux, em seguida instalando o editor VIM, caso necessitar de editar algum código no Shell.

Montar a Imagem, importante estar dentro do diretório que foi criado, e o Dockerfile estar dentro do mesmo diretório:
```
docker build -t apache-docker .
```
Detalhe o sinal de ponto, ao final, informar para pegar tudo que esta dentro do diretório.
É colocado uma tag de identificação para melhot localização, com -t , pode ser o nome de sua preferência. 

Será mostrado mensagens da evolução do processo como abaixo:
```
[+] Building 1.1s (10/10) FINISHED                                                                                                         
 => [internal] load build definition from Dockerfile                                                                                  0.2s
 => => transferring dockerfile: 37B                                                                                                   0.0s
 => [internal] load .dockerignore                                                                                                     0.3s
 => => transferring context: 2B                                                                                                       0.0s
 => [internal] load metadata for docker.io/library/httpd:2.4                                                                          0.3s
 => [auth] library/httpd:pull token for registry-1.docker.io                                                                          0.0s
 => [internal] load build context                                                                                                     0.1s
 => => transferring context: 60B                                                                                                      0.0s
 => [1/4] FROM docker.io/library/httpd:2.4@sha256:a182ef2350699f04b8f8e736747104eb273e255e818cd55b6d7aa50a1490ed0c                    0.0s
 => CACHED [2/4] COPY ./ /usr/local/apache2/htdocs/                                                                                   0.0s
 => CACHED [3/4] RUN ["apt-get", "update"]                                                                                            0.0s
 => CACHED [4/4] RUN ["apt-get", "install", "-y", "vim"]                                                                              0.0s
 => exporting to image                                                                                                                0.2s
 => => exporting layers                                                                                                               0.0s
 => => writing image sha256:d0b5d542ef584608a2b54597c2a3b85dd5ab06814ab7b349f38d2b0cf279b61d                                          0.0s
 => => naming to docker.io/library/apache-docker                                           0.0s
```
Verificar se existe a imagem no sistema:
```
$ docker images
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
apache-docker   latest    d0b5d542ef58   16 minutes ago   199MB
```
Neste momento já temos a nossa imagem no sistema, precisamos agora rodar ela em um Docker container.
Verificar agora se existe algum contâiner rodando:
```
$ docker ps
```
Se não existir nenhum container rodando, vai ser retornado uma tabela vazia:
```
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
Agora vamos colocar para rodar um container pegando como argumento o ID da imagem Docker,
como podemos conferir pelo comando : docker images, feito acima:
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
apache-docker   latest    d0b5d542ef58   16 minutes ago   199MB
Pegamos a coluna IMAGE ID , como argumento para o comando abaixo:
docker run -dit --name NOME_BATISMO_CONTAINER -p 80:80 IMAGE ID 
```
$ docker run -dit --name apache-docker -p 80:80 d0b5d542ef58
```
Vai ser montado na porta 80 local, comunicando com a porta 80 do container.

Após isto será exibido a mensagem que a porta 80 está sendo acionada.  <br/> 
![Imagem Port open codespace github](/images/Port_open_codespace.jpg)  <br/>

Ao clicar ou seguir a porta 80 será exibida pagina index.html, que foi criada anteriormente.  <br/>
![Imagem Pagina Index.html](/images/pagina_funcionou.jpg)  <br/>
[Topo](#ancora)


<a id="acrescentareditar"></a>
## 📁 Acrescentar e Editar arquivos

> Para acrescentar e editar arquivos no servidor, precisamos entrar no docker container.
> Vamos acessar ele através de seu bash.Para isto utilizamos o comando 'exec' do docker.
```
 $ docker exec -it 1e936a216edd bash
```
Vai aparecer a seguir a linha de comando bash do container docker.
**O simbolo '#' é o seu status de usuario, que indica ser root, o que terá de ser digitado é somente o texto que está após este sinal.
Se existir conteúdo abaixo do console é somente a visualização da saida do comando.**
```
root@1e936a216edd:/usr/local/apache2#
```
Vamos só para visualizar o conjunto. Listar os arquivos:
```
root@1e936a216edd:/usr/local/apache2# ls
bin  build  cgi-bin  conf  error  htdocs  icons  include  logs  modules
```
Abrir a pasta publica e a seguir listar os arquivos desta pasta.Vamos encontrar os arquivos que criamos da pasta da maquina local.
```
root@1e936a216edd:/usr/local/apache2# cd htdocs
root@1e936a216edd:/usr/local/apache2/htdocs# ls
Dockerfile  index.html
```

Agora vamos entrar no arquivo que queremos editar, ou criar um novo, aí fica conforme a demanda de seu projeto ou  trabalho.
Para isso vamos usar o VIM, que é um dos primeiros editores Shell nativo. Explicaremos aqui comandos básicos para editar e salvar seus documentos.Se quiser buscar [informações completas](https://www.vim.org/).  </br>

Ao abrir o VIM parece uma coisa meio estranha, pois é  um editor feito para ambiente de linha de comando, e os usuarios acostumados com editores graficos ficam bem confusos. Ao iniciar o VIM aguarda oque o usuário deseja fazer, pode ser edição,  visualização ou navegação. Estes comandos são  divididos em comandos de exibição, edição e navegação.
Vamos editar agora o nosso arquivo index.html, para isto digite:
```
root@1e936a216edd:/usr/local/apache2/htdocs#  vi index.html
```

Então o arquivo index.html é exibido, assim:

```
colocar imagem aqui
```
Repare no rodapé,  é nesta área da interface do programa que precisamos prestar atenção, aí é mostrado qual a ação que estamos fazendo.
 Vamos iniciar a edição. A tecla esc funciona como uma baliza, em uma analogia mais simplório é como fosse o ponto neutro do câmbio de um carro, mais para frente vai ficar claro esta comparação.

aperte a tecla esc, para ter certeza que está no menu básico. Em seguida aperte a tecla 'e'. No rodapé do editor aparecerá a palavra INSERT, significando que está  no modo de edição.

Você pode clonar este repositório:

```bash
# Clone este repositório
$ git clone <https://github.com/joseivangeraldo/html_css>
```

MIT License

Copyright (c) <2023> <José Ivan G Silva>


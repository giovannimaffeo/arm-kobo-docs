# arm-kobo-docs

Este repositório contém a documentação detalhada e os códigos-fonte dos repositórios do KoboToolbox adaptados para a arquitetura ARM, permitindo que a aplicação seja executada em dispositivos como o Raspberry Pi 4. A solução foi desenvolvida com o objetivo de oferecer uma alternativa de baixo custo e eficiente para ambientes com restrições de hardware, mantendo todas as funcionalidades do KoboToolbox.

## Sobre o KoboToolbox

O KoboToolbox é uma plataforma de código aberto amplamente utilizada por organizações humanitárias, institutos de pesquisa e instituições governamentais para a coleta, gerenciamento e análise de dados em campo. Desenvolvida para ambientes com conectividade limitada, a aplicação suporta tanto a coleta de dados offline quanto online, permitindo a administração de grandes volumes de informações de forma eficiente. Seus principais módulos incluem ferramentas para criação de formulários, preenchimento em interfaces web (Enketo) e integração com aplicativos móveis (KoBoCollect). Mais informações podem ser encontradas no [repositório oficial do KoboToolbox](https://github.com/kobotoolbox).

A adaptação desta solução para ARM visa possibilitar que a plataforma rode em dispositivos mais acessíveis, como o Raspberry Pi, sem comprometer a robustez e a funcionalidade do KoboToolbox. Essa adaptação é especialmente valiosa em projetos com recursos limitados ou em contextos onde o uso de servidores tradicionais pode não ser viável.

## Visão Geral

O objetivo deste repositório é fornecer um ambiente completo do KoboToolbox com suporte à arquitetura ARM. Isso inclui a refatoração de diversos módulos da aplicação e a recompilação das imagens Docker para garantir compatibilidade com dispositivos ARM. Abaixo estão listados os repositórios alterados e suas respectivas funcionalidades.

### Repositórios e Módulos Alterados

| Módulo             | Repositório ARM Adaptado                                             | Descrição                                                                                  |
|--------------------|----------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| kobo-install       | [arm-kobo-install](https://github.com/giovannimaffeo/arm-kobo-install) | Ferramenta para configuração e orquestração dos containers do KoboToolbox.                  |
| enketo-express     | [arm-kobo-enketo-express](https://github.com/giovannimaffeo/arm-kobo-enketo-express) | Interface web para preenchimento de formulários.                                           |
| kpi                | [arm-kobo-kpi](https://github.com/giovannimaffeo/arm-kobo-kpi)       | Módulo responsável pela criação e gerenciamento de formulários.                             |
| nginx-certbot      | [arm-kobo-nginx-certbot](https://github.com/giovannimaffeo/arm-kobo-nginx-certbot) | Configuração do NGINX com suporte HTTPS via Let's Encrypt.                                  |
| kobo-docker        | [arm-kobo-docker](https://github.com/giovannimaffeo/arm-kobo-docker) | Repositório principal com as configurações de containers e serviços do KoboToolbox.        |
| kobocat            | [kobocat (repositório oficial)](https://github.com/kobotoolbox/kobocat) | Módulo de backend responsável pelo armazenamento de respostas. (Nenhuma alteração foi necessária) |

## Instruções de Instalação

### 3. Atualizar Código Local

1. Criar um diretório chamado `kobo`.
2. Clonar os repositórios ARM adaptados abaixo:
   - [kobo-install](https://github.com/giovannimaffeo/arm-kobo-install)
   - [enketo-express](https://github.com/giovannimaffeo/arm-kobo-enketo-express)
   - [kpi](https://github.com/giovannimaffeo/arm-kobo-kpi)
   - [nginx-certbot](https://github.com/giovannimaffeo/arm-kobo-nginx-certbot)
   - [kobo-docker](https://github.com/giovannimaffeo/arm-kobo-docker)
   - [kobocat](https://github.com/kobotoolbox/kobocat) (repositório oficial)

3. Dentro do Raspberry Pi, para cada repositório clonado, execute:
```bash
git checkout dev
```

### 4. Compilar as Imagens para ARM

Será necessário compilar as imagens dos seguintes módulos:
- enketo-express
- kpi
- kobocat

Para compilar as imagens, execute os comandos abaixo para cada repositório correspondente:
```bash
cd <repositório>
docker build . -t <nome-módulo>-arm
```

Certifique-se de que o nome das imagens seja exatamente como mostrado para garantir a integração correta no setup.

### 5. Configurar KoboToolbox

1. Acesse o diretório `kobo-install`:
```bash
cd kobo/kobo-install
```
2. Execute o setup:
```bash
python3 run.py --setup
```

### Detalhamento das Alterações dos Módulos do KoboToolbox

Algumas adaptações foram necessárias para garantir a execução do KoboToolbox na arquitetura ARM. Essas modificações envolvem principalmente ajustes nos arquivos Dockerfiles e Docker Compose para garantir a compatibilidade e estabilidade da aplicação em dispositivos como o Raspberry Pi. A seguir, estão descritas as principais alterações realizadas para cada módulo.

#### 1. Enketo Express 

No módulo Enketo Express, responsável pela interface web de preenchimento de formulários, foi necessário incluir o pacote `chromium` para garantir compatibilidade com a arquitetura ARM. Esse pacote é essencial para rodar certas funcionalidades baseadas em navegadores. A imagem mostra a modificação feita no Dockerfile do módulo, onde a linha adicionada garante a instalação correta desse pacote.

![image 16](https://github.com/user-attachments/assets/bf00a24f-d610-4073-a4f0-c5f227ae17e3)

#### 2. Kobo Docker 

No repositório principal do KoboToolbox, foi necessário modificar os arquivos Docker Compose para substituir as imagens padrão por imagens compatíveis com ARM. A segunda e terceira imagens ilustram a mudança nas referências de imagem de módulos críticos, como `kobocat`, `kpi` e `enketo-express`, para suas versões ARM.

![image 17](https://github.com/user-attachments/assets/6cf450ec-b210-44f1-8d52-aa28465ce0b9)

![image 18](https://github.com/user-attachments/assets/f98080b0-7b18-4a47-9754-aff1cb648f52)

#### 3. KPI

No módulo KPI, utilizado para a criação e gerenciamento de formulários, foi necessário adicionar manualmente a instalação de diversos pacotes para garantir que o build funcionasse corretamente na arquitetura ARM. A quarta imagem destaca essas adições ao Dockerfile do KPI, garantindo que todas as dependências sejam instaladas corretamente durante o processo de compilação.

![image 19](https://github.com/user-attachments/assets/5f37881b-b951-4bd1-9654-133891976721)

#### 4. NGINX Certbot

Para o módulo NGINX Certbot, que configura o suporte HTTPS utilizando o Let’s Encrypt, foram feitas adaptações no script de inicialização para considerar os subdomínios configurados para a arquitetura ARM. A quinta imagem mostra a adaptação das configurações para permitir o funcionamento correto da aplicação em ambientes ARM.

![image 20](https://github.com/user-attachments/assets/ce343ff8-ae5b-4253-bb7d-0fc79bcbf817)

Essas alterações garantem que todos os módulos principais do KoboToolbox rodem de forma estável em dispositivos ARM, permitindo sua utilização em ambientes com hardware limitado, como o Raspberry Pi.

---

Este repositório oferece uma solução prática e eficiente para quem deseja rodar o KoboToolbox em arquitetura ARM, mantendo as mesmas funcionalidades da arquitetura original.

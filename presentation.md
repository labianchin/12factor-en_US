name: 12factor
class: center, middle, inverse

# 12 Factor App

### Construindo aplicações para escala da web

.hidden[
With powerful tools and modern cloud platforms one can think on how to properly build web apps that suits these environments. The 12  Factor App is a set of best practices on how apps built for PaaS should be architected. At its core, apps need not to care where they are.
]

---

## Motivação

- Heroku PaaS com a stack Cedar como uma forma de construir apps portáveis
- Fácil de implantar e entregar, para qualquer linguagem
- Docker, CoreOS, Dokku, Flynn, Deis, ...
- 12 Factor: Boas praticas de como apps para PaaS devem ser arquitetadas
- Apps PaaS-friendly não devem se importar onde estão rodando

???

- IaaS => Infra, PaaS => Apps


---

## Introdução

- Formatos **declarativos** de automação
- **Máxima portabilidade** entre ambientes de execução
- **Implantação** em modernas **plataformas cloud**
- **Minimização de divergência** entre prod/dev
- Permite **implantação contínua**
- **Escalabilidade** fácil

---

## I. Base de código

Uma base de código num sistema de controle de versões, muitas implantações (deploys)

- Única app por base de código

.center[ ![]( http://12factor.net/images/codebase-deploys.png ) ]

???

- Easy to track what code is deployed


---

## II. Dependências

Declare e isole dependências explicitamente

- Nunca confie na existência implícita de ferramentas do sistema
- Suporte de builds reproduzíveis
- Exemplo de ferramentas: gem/bundle, pip/virtualenv, autoconf, ...

???

ALTA importância

---

## III. Configuração

Guarde a configuração no ambiente (NÃO no código)

- Configuração é qualquer coisa que pode variar entre implantações:
  - Controladores de recursos
  - Credenciais
  - Hostnames canônicos por implantação
- Separação estrita entre configuração de código
- Não incluí configuração interna da aplicação (ex: Spring)

---

## IV. Serviços de apoio

Trate serviços de apoio como recursos anexados

- Sem distinção entre serviços locais e de terceiros
- Permite grande flexibilidade
- Baixo acoplamento a implantação
- Recursos podem ser alterados em implantações á vontade, sem mudanças de código

.center.fixsize[ ![](http://12factor.net/images/attached-resources.png) ]

???

- Possible to use a fake service for testing
- Easy to change database servers or e-mail providers


HIGH importance

---

## V. Build, release, run

Estrita separação entre estágios de build, release e run

- **Build** : Converte código do repositório para um pacote executável
- **Release** : Build com configuração da implantação, pronto para execução imediata
- **Run** : Lança um conjunto de processos contra um release selecionado

.center[ ![](http://12factor.net/images/release.png) ]

???

- Example tool: Capistrano
- Run stage -> simple
- Build -> more complex: errors are visible for the devs
- Conceptual importance. The tools you use shape these processes.


---

## VI. Processos

Execute a aplicação como um ou mais processos sem estado

- Stateless e share-nothing
- Dados de sessão devem ser guardados em outro serviço, ex: *Memcached* ou *Redis*
- Sem estado significa:
 - Mais robusto
 - Fácil de gerenciar
 - Menos bugs
 - Escala melhor

???

- The app is executed in the execution environment
- Easy to tear down and move to other server

HIGH importance

---

## VII. Vínculo de porta

Exporte serviços através de um vínculo de uma porta

- Exporte HTTP como um serviço através de uma porta
- Camada de roteamento para lidar com pedidos de encaminhamento a um hostname 
- Use bibliotecas de webserver como Jetty para JVM ou Thin para Ruby 
- Um app pode se tornar o serviço de apoio para outro app


---

## VIII. Concorrência

Escale pelo o modelo de processo 

- Processos são cidadões de primeira classe 
- Nunca daemonize ou escreva arquivos PID 
- Confie num gerente processos (upstart, systemd, launchd, foreman, ...) para: 
  - Gerenciar fluxos de saída 
  - Responder a processos travados 
  - Controle de reinícios e desligamentos

???

- Foreman, good tool, can export to other processes managers


---

## IX. Descartabilidade

Maximizar a robustez com rápida inicialização e desligamento

- .hidden[ Os processos de apps são descartáveis: ] Podem ser iniciados ou interrompido em qualquer momento
- Desligar elegantemente (gracefully) quando receber um sinal SIGTERM
- Os processos devem ser robustos contra morte súbita

.hidden[
- Crash-only software
 Importance: Medium Depending on how often you are releasing new code (hopefully many times per day, if you can), and how much you have to scale your app traffic up and down on demand, you probably won’t have to worry about your startup/shutdown speed, but be sure to understand the implications for your app. 
]


---

## X. Paridade entre Dev/prod

Mantenha ambientes (dev, staging, prod) o mais similar possível

- Projetado para implantação contínua
- Manter a lacuna entre dev e prod pequena:
 - Lacuna de tempo
 - Lacuna de pessoas
 - Lacuna de ferramentas
- Resistir a usar diferentes serviços entre dev e prod

???

- Muito tempo para chegar em prod
- devs codificam, ops implantam
- diferentes ferramentas entre dev/prod


---

## XI. Logs

Trate logs como fluxo de eventos

- Fluxo de eventos é escrito para STDOUT
- Use roteadores de logs, como Logplex e Fluent

.hidden[
Note: writing to stdout seems very purist
]

---

## XII. Processos de Admin

Rode tarefas de admin/gerenciamento como processos pontuais

- Rode contra um release: mesmo código e configuração como outros processos
- Deve ser entregue com o código de aplicação evitando problemas de sincronização
- ex: Migrações de banco de dados


.hidden[ Importance: HIGH Having console access to a production system is a critical administrative and debugging tool, and every major language/framework provides it. No excuses for sloppiness here ]

---

## Fontes

- http://12factor.net/
- https://news.ycombinator.com/item?id=3267187
- http://www.clearlytech.com/2014/01/04/12-factor-apps-plain-english/
- http://www.coderanch.com/t/626165/java/java/factor-app-principles-apply-Java
- https://blog.appfog.com/docker-and-the-future-of-the-paas-layer/

---

class: center, middle, inverse

## Perguntas?


# 🌐 Guia de Arquitetura Web: HTTP, URLs, DNS, TLS e Evolução do Protocolo

Material de estudo organizado para consolidar os fundamentos de comunicação web, saindo da base de redes até HTTP/3.

> 📚 Este conteúdo foi estruturado para progressão didática: **fundamentos → funcionamento prático → segurança → performance e evolução**.

---

## 📑 Índice

1. [Fundamentos da comunicação na internet](#1-fundamentos-da-comunicação-na-internet)
2. [Modelos de arquitetura: Cliente-Servidor e P2P](#2-modelos-de-arquitetura-cliente-servidor-e-p2p)
3. [URLs, URIs e URNs](#3-urls-uris-e-urns)
4. [Portas, domínios, DNS e IP](#4-portas-domínios-dns-e-ip)
5. [HTTP na prática: requisição, resposta e status code](#5-http-na-prática-requisição-resposta-e-status-code)
6. [Headers HTTP, autenticação e estado](#6-headers-http-autenticação-e-estado)
7. [HTTP vs HTTPS, TLS e certificados](#7-http-vs-https-tls-e-certificados)
8. [Fluxo completo de uma requisição HTTPS](#8-fluxo-completo-de-uma-requisição-https)
9. [Evolução do protocolo: HTTP/1.1, HTTP/2 e HTTP/3](#9-evolução-do-protocolo-http11-http2-e-http3)
10. [Resumo final](#10-resumo-final)
11. [Itens essenciais para Dev Jr](#11-itens-essenciais-para-dev-jr)

---

## 1. Fundamentos da comunicação na internet

### 📌 O que é HTTP?

**HTTP (HyperText Transfer Protocol)** é um protocolo da camada de aplicação usado para comunicação entre cliente e servidor.

Ele define regras para troca de mensagens na web.

### 🧠 O que é um protocolo?

Um protocolo é um conjunto de regras para comunicação.

Em termos simples:
- uma parte envia uma mensagem;
- outra parte responde;
- ambas seguem um formato e um comportamento predefinidos.

Sem protocolo, a comunicação falha.

### 🧱 Camadas da internet (visão simplificada)

A comunicação HTTP acontece dentro de uma pilha maior:

1. **Camada Física**
   - Cabos, Wi-Fi, 4G/5G, infraestrutura física
2. **Camada de Enlace**
   - Comunicação dentro da rede local
3. **Camada de Rede**
   - Endereçamento IP e roteamento
4. **Camada de Transporte**
   - TCP e UDP
   - O HTTP tradicionalmente opera sobre TCP
5. **Camada de Aplicação**
   - HTTP, navegadores, APIs e apps

> ✅ Ponto-chave: o HTTP está na aplicação, mas depende das camadas inferiores para entregar dados.

---

## 2. Modelos de arquitetura: Cliente-Servidor e P2P

### 🖥️ Modelo Cliente-Servidor

No modelo clássico da web:
- o **cliente** inicia a comunicação;
- o **servidor** processa e responde.

Fluxo básico:

```text
Cliente -> Requisição -> Servidor
Servidor -> Resposta -> Cliente
```

#### Quem pode ser cliente?
- Navegador (Chrome, Firefox, etc.)
- Aplicativo mobile
- Sistema que faz requisição HTTP

#### Quem pode ser servidor?
- Aplicação que recebe requisições
- Processa regras de negócio
- Retorna dados (HTML, JSON, arquivos etc.)

### 🧩 Arquitetura típica do projeto

Cenário comum em aplicações web modernas:

1. Cliente (browser)
2. Servidor de front-end
3. Servidor de back-end

Fluxo:
1. O navegador requisita recursos ao front-end.
2. O front-end entrega HTML/CSS/JS.
3. O navegador chama APIs no back-end.
4. O back-end processa e retorna resposta.

### 🔄 Modelo P2P (Peer-to-Peer)

No P2P, não há separação fixa entre cliente e servidor.
Cada nó pode consumir e fornecer recursos ao mesmo tempo.

Exemplo clássico: **BitTorrent**.

### ⚔️ Cliente-Servidor vs P2P

| Aspecto | Cliente-Servidor | P2P |
|---|---|---|
| Estrutura | Centralizada | Distribuída |
| Processamento | Concentrado no servidor | Distribuído entre peers |
| Escalabilidade | Pode ter gargalo | Escala melhor em vários cenários |
| Controle | Mais simples | Mais complexo |
| Segurança e governança | Mais centralizadas | Mais desafiadoras |

### ✅ Quando usar cada modelo?

**Cliente-Servidor**:
- aplicações web tradicionais;
- APIs centralizadas;
- necessidade de controle forte.

**P2P**:
- compartilhamento de arquivos grandes;
- sistemas distribuídos;
- cenários colaborativos e descentralizados.

---

## 3. URLs, URIs e URNs

### 🌍 O que é URL?

**URL (Uniform Resource Locator)** é o endereço de um recurso na rede.

Exemplo:

```text
https://localhost:3000/
```

Estrutura geral:

```text
protocolo://host:porta/caminho
```

Partes:
- **Protocolo**: `http`, `https`, `ftp`, `ssh`...
- **Host**: domínio ou IP (`localhost`, `meusite.com`)
- **Porta**: ponto lógico do serviço (`80`, `443`, `3000`...)
- **Caminho**: recurso dentro do servidor (`/`, `/login`, `/produtos/123`)

Exemplo com query string:

```text
https://api.site.com/produtos?categoria=backend&pagina=2
```

- `?categoria=backend&pagina=2` representa os **query params** (filtros e paginação, por exemplo).

### 📦 O que pode ser um recurso?

Uma URL pode apontar para:
- página HTML;
- CSS, JS, imagem;
- endpoint de API;
- JSON;
- arquivo de download.

### 🔎 URI vs URL vs URN

#### URI (Uniform Resource Identifier)
Conceito mais amplo: identifica um recurso.

#### URL (Uniform Resource Locator)
Tipo de URI que identifica **e localiza** o recurso (incluindo protocolo/endereço).

#### URN (Uniform Resource Name)
Tipo de URI que apenas nomeia o recurso, sem indicar onde ele está.

Exemplo URN:

```text
urn:cursos:alura:course:introducao-html-css
```

### 📊 Relação entre os termos

```text
URI
├── URL
└── URN
```

Tabela comparativa:

| Tipo | Identifica | Localiza | Define protocolo |
|---|---|---|---|
| URI | Sim | Pode ou não | Pode ou não |
| URL | Sim | Sim | Sim |
| URN | Sim | Não | Não |

> 💡 No uso diário, URL e URI costumam ser tratados como sinônimos. Tecnicamente, URL é um subtipo de URI.

---

## 4. Portas, domínios, DNS e IP

### 🔌 O que é porta?

Porta é uma entrada lógica de um serviço dentro de uma máquina.

Um mesmo host pode executar vários serviços ao mesmo tempo, cada um em uma porta diferente.

Exemplos:
- Front-end local: `http://localhost:3000`
- Back-end local: `http://localhost:8000`

### 🚦 Portas padrão

| Porta | Uso |
|---|---|
| 80 | HTTP |
| 443 | HTTPS |
| 0-1023 | Faixa reservada para serviços conhecidos |
| 1024-65535 | Faixa livre para aplicações |

O navegador costuma ocultar a porta quando ela é padrão (`:80` em HTTP e `:443` em HTTPS).

### 🌐 Domínio vs IP

Todo servidor tem um endereço IP, por exemplo:

```text
142.251.128.14
```

Mas pessoas usam nomes amigáveis, como:

```text
google.com
```

### 🔎 O que é DNS?

**DNS (Domain Name System)** traduz nome de domínio para IP.

Exemplo de tradução:

```text
google.com -> 142.251.128.14
```

Consulta comum no terminal:

```bash
nslookup google.com
```

### 🏗️ Hierarquia do DNS (simplificada)

```text
.
├── com
│   └── google
├── br
│   ├── com
│   │   └── alura
│   ├── gov
│   └── edu
├── org
└── net
```

### 🌍 Domínio global vs local

- **Global**: `google.com`, `alura.com.br`
- **Local**: `localhost` (válido apenas na máquina local)

---

## 5. HTTP na prática: requisição, resposta e status code

### 📤 Estrutura de uma requisição HTTP

Uma requisição geralmente inclui:
- método (`GET`, `POST`, `PUT`, `DELETE`...);
- URL;
- headers;
- body (opcional).

Exemplo:

```http
GET /produtos HTTP/1.1
Host: api.site.com
Accept: application/json
```

### 📥 Estrutura de uma resposta HTTP

Uma resposta inclui:
- status code (`200`, `404`, `500`...);
- headers;
- body.

Exemplo:

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 85

{"ok": true}
```

### 🔢 Status codes comuns

| Código | Significado |
|---|---|
| 200 | OK |
| 201 | Criado |
| 400 | Requisição inválida (erro do cliente) |
| 404 | Não encontrado |
| 500 | Erro interno do servidor |

Classes de status code:
- **1xx**: informacional
- **2xx**: sucesso
- **3xx**: redirecionamento
- **4xx**: erro do cliente
- **5xx**: erro do servidor

### 🧭 Métodos HTTP mais usados (na prática)

| Método | Uso comum | Body | Idempotente |
|---|---|---|---|
| GET | Buscar recurso(s) | Normalmente não | Sim |
| POST | Criar recurso / ação | Sim | Não |
| PUT | Atualizar recurso completo | Sim | Sim |
| PATCH | Atualização parcial | Sim | Depende da implementação |
| DELETE | Remover recurso | Opcional | Sim |

> 💡 Idempotente significa: repetir a mesma operação várias vezes mantém o mesmo resultado final no servidor.

---

## 6. Headers HTTP, autenticação e estado

### 📦 O que são headers?

Headers HTTP são metadados da requisição/resposta, no formato:

```text
Chave: Valor
```

Exemplos:

```text
Content-Type: application/json
Content-Length: 120
Authorization: Bearer <token>
```

Eles transportam informações como:
- formato de dados;
- autenticação;
- cache;
- idioma;
- cookies.

### 🔁 Tipos principais

#### Request headers (cliente -> servidor)

```http
GET /users HTTP/1.1
Host: api.site.com
Authorization: Bearer token123
Content-Type: application/json
Accept: application/json
```

#### Response headers (servidor -> cliente)

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 120
Set-Cookie: sessionId=abc123
```

### 🔑 Header Authorization

Formato mais comum em APIs:

```text
Authorization: Bearer <token>
```

`Bearer` significa “portador”: quem possui o token válido pode acessar o recurso autorizado.

### 🧠 HTTP é stateless

O protocolo HTTP não mantém estado entre requisições por padrão.
Cada requisição é independente.

Por isso, identidade/sessão do usuário precisa ser enviada continuamente.

### 🧩 Formas de manter contexto do usuário

1. **Token** (muito comum em APIs)
   - login gera token;
   - cliente envia token em cada requisição;
   - servidor valida token.
2. **Sessão**
   - servidor cria `session_id` após login;
   - cliente envia o identificador nas próximas requisições.
3. **Cookies**
   - servidor envia `Set-Cookie`;
   - navegador armazena e reenvia automaticamente em `Cookie`.

### 🗂️ Headers comuns

| Header | Função |
|---|---|
| Authorization | Autenticação |
| Content-Type | Tipo de conteúdo |
| Content-Length | Tamanho do conteúdo |
| Accept | Formato de resposta aceito |
| User-Agent | Informações do cliente |
| Cookie | Dados enviados pelo navegador |
| Set-Cookie | Instrução para criar cookie |

### 🌐 CORS (essencial no dia a dia)

**CORS (Cross-Origin Resource Sharing)** é a política que controla quando um front-end pode acessar uma API de outro domínio/porta/protocolo.

Exemplo comum de erro em desenvolvimento:
- Front-end em `http://localhost:3000`
- API em `http://localhost:8000`

Mesmo sendo localhost, as origens são diferentes (porta diferente), então o navegador aplica regras de CORS.

Header de resposta típico para liberar acesso:

```http
Access-Control-Allow-Origin: http://localhost:3000
```

---

## 7. HTTP vs HTTPS, TLS e certificados

### ⚔️ HTTP vs HTTPS

| Característica | HTTP | HTTPS |
|---|---|---|
| Segurança | Não criptografado | Criptografado |
| Porta padrão | 80 | 443 |
| Uso recomendado | Testes locais e cenários controlados | Produção e dados sensíveis |

### 🔐 O que é TLS?

**TLS (Transport Layer Security)** protege a comunicação com:
- **Confidencialidade**: terceiros não leem os dados;
- **Integridade**: dados não são alterados no trajeto;
- **Autenticidade**: cliente verifica a identidade do servidor.

### 📜 Certificado digital

Documento apresentado pelo servidor contendo, entre outros dados:
- domínio;
- chave pública;
- autoridade certificadora (CA);
- validade.

Exemplos de CAs: Let's Encrypt, DigiCert.

### 🔑 Chave pública e chave privada

- **Chave pública**: pode ser compartilhada; usada para criptografar.
- **Chave privada**: permanece no servidor; usada para descriptografar.

### 🤝 Handshake TLS (visão prática)

1. Cliente inicia conexão segura.
2. Servidor envia certificado com chave pública.
3. Cliente valida certificado.
4. Cliente e servidor negociam uma chave de sessão.
5. A partir daí, comunicação segue criptografada (normalmente com criptografia simétrica, mais rápida).

> 🔒 Em produção, sempre que houver senha, token ou dado pessoal, HTTPS é obrigatório.

---

## 8. Fluxo completo de uma requisição HTTPS

Visão geral:

```text
Usuário digita URL
      |
      v
DNS resolve domínio -> IP
      |
      v
Conexão TCP
      |
      v
Handshake TLS
      |
      v
Requisição HTTP (criptografada)
      |
      v
Resposta HTTP (criptografada)
      |
      v
Browser renderiza conteúdo
```

### 🔍 Etapas detalhadas

1. **DNS**: descobre o IP do domínio.
2. **TCP**: abre conexão confiável (SYN, SYN-ACK, ACK).
3. **TLS**: valida certificado e estabelece criptografia.
4. **HTTP sobre canal seguro**: troca request/response.
5. **Renderização**: navegador processa HTML, CSS, JS e dados.

Resumo por etapa:

| Etapa | O que acontece |
|---|---|
| DNS | Resolução de domínio para IP |
| TCP | Estabelecimento da conexão |
| TLS | Segurança da comunicação |
| HTTP | Troca de dados de aplicação |

---

## 9. Evolução do protocolo: HTTP/1.1, HTTP/2 e HTTP/3

### HTTP/1.1

Características:
- formato textual;
- keep-alive (reuso de conexão);
- limitações de paralelismo;
- headers sem compressão eficiente.

Problema clássico: **head-of-line blocking** em cenários de fila/espera.

### HTTP/2

Principais melhorias:
- **multiplexing** (várias requisições na mesma conexão);
- protocolo binário;
- compressão de headers (HPACK);
- melhor aproveitamento da conexão.

Comparação rápida:

| Característica | HTTP/1.1 | HTTP/2 |
|---|---|---|
| Formato | Texto | Binário |
| Multiplexing | Não | Sim |
| Compressão de headers | Não | Sim |
| Performance | Média | Alta |

> ℹ️ Na prática, HTTP/2 é geralmente utilizado com TLS.

### HTTP/3 (QUIC)

Grande mudança: sai TCP, entra **QUIC sobre UDP**.

Vantagens principais:
- streams independentes (reduz bloqueios por perda de pacote);
- menor latência na conexão;
- TLS integrado ao transporte QUIC;
- melhor experiência em redes móveis e instáveis.

Comparativo geral:

| Característica | HTTP/1.1 | HTTP/2 | HTTP/3 |
|---|---|---|---|
| Protocolo base | TCP | TCP | UDP (QUIC) |
| Multiplexing | Não | Sim | Sim |
| Head-of-line blocking | Alto | Ainda existe no nível TCP | Mitigado |
| TLS | Opcional | Comum | Embutido no QUIC |
| Performance | Média | Alta | Muito alta (especialmente em redes reais) |

---

## 10. Resumo final

### ✅ Pontos essenciais

- HTTP é um protocolo da camada de aplicação baseado em modelo cliente-servidor.
- URL é o endereço do recurso; URI é o conceito mais amplo; URN apenas nomeia.
- DNS traduz domínio em IP.
- Portas identificam serviços dentro do host (80/443 são padrões web).
- Requisição e resposta HTTP têm estrutura bem definida (linha inicial, headers e body).
- Headers são fundamentais para autenticação, formato de dados, cache e cookies.
- HTTP é stateless; tokens/sessões/cookies ajudam a manter contexto do usuário.
- HTTPS = HTTP com TLS, garantindo confidencialidade, integridade e autenticidade.
- A web moderna evoluiu de HTTP/1.1 para HTTP/2 e HTTP/3 (QUIC), com ganhos de desempenho e resiliência.

### 🚀 Conclusão

Esse conjunto de conceitos forma a base para compreender como aplicações web funcionam em produção: desde endereçamento e transporte até segurança e performance.

---

## 11. Itens essenciais para Dev Jr

### ✅ Checklist rápido

- Entender diferença entre `GET`, `POST`, `PUT`, `PATCH` e `DELETE`.
- Saber ler status code por classe (`2xx`, `4xx`, `5xx`).
- Identificar partes da URL: protocolo, host, porta, caminho e query params.
- Saber por que CORS acontece e como resolver no servidor.
- Usar HTTPS em produção sempre que houver autenticação ou dados sensíveis.
- Reconhecer que HTTP é stateless e exige token/sessão/cookie para contexto de usuário.

### 🧪 Ferramentas úteis para treino

- Browser DevTools (aba Network)
- `curl` no terminal
- Postman ou Insomnia

Exemplo com `curl`:

```bash
curl -i http://localhost:8000/users
```

O `-i` mostra status line e headers da resposta, ótimo para estudar HTTP na prática.

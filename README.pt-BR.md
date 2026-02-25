<p align="center">
  <img src="https://github.com/user-attachments/assets/c16e9208-a4ce-459c-97e9-6a9f95b2f159" width="200" alt="Juntos Somos Mais">
</p>

# &lt;backend-challenge /&gt;

*[Read in English](./README.md)*

O principal objetivo deste desafio é avaliar sua abordagem para **resolução de problemas, qualidade de código e como você utiliza ferramentas modernas** — incluindo IA.

Avaliamos:

- Seu estilo de código e organização
- Tomada de decisão e trade-offs
- Estratégias de teste
- Qualidade da documentação
- Como você usa IA como ferramenta de desenvolvimento

> 🤖 **IA é bem-vinda aqui.** Não queremos saber *se* você usou IA. Queremos saber *como* você usou.

---

## Índice

- [O Desafio](#o-desafio)
- [Regras de Negócio](#regras-de-negócio)
- [Requisitos da API](#requisitos-da-api)
- [Critérios de Avaliação](#critérios-de-avaliação)
- [Jornada IA (Obrigatório)](#jornada-ia-obrigatório)
- [Entrega](#entrega)
- [FAQ](#faq)

---

## O Desafio

Recebemos dados de clientes de empresas parceiras nos formatos **CSV** e **JSON**. Sua tarefa é:

1. **Carregar** dados de URLs externas na inicialização da aplicação
2. **Transformar** os dados aplicando nossas regras de negócio
3. **Expor** uma API REST para consultar os dados processados

### Dados de Entrada

| Formato | URL | Registros |
|---------|-----|-----------|
| CSV | [input-backend.csv](https://storage.googleapis.com/juntossomosmais-code-challenge/input-backend.csv) | ~1000 |
| JSON | [input-backend.json](https://storage.googleapis.com/juntossomosmais-code-challenge/input-backend.json) | ~1000 |

> ⚠️ Os dados devem ser carregados via requisição HTTP **na inicialização** e mantidos **em memória**. Não é necessário banco de dados.

---

## Regras de Negócio

### 1. Classificação de Clientes por Localização

Baseado nas coordenadas, classificar cada cliente:

| Tipo | Bounding Box |
|------|--------------|
| **ESPECIAL** | minlon: -2.196998, minlat: -46.361899, maxlon: -15.411580, maxlat: -34.276938 |
| **ESPECIAL** | minlon: -19.766959, minlat: -52.997614, maxlon: -23.966413, maxlat: -44.428305 |
| **NORMAL** | minlon: -26.155681, minlat: -54.777426, maxlon: -34.016466, maxlat: -46.603598 |
| **TRABALHOSO** | Qualquer um que não se encaixe nas regras acima |

### 2. Transformações de Dados

| Campo | Transformação |
|-------|---------------|
| `phone`, `cell` | Converter para formato [E.164](https://en.wikipedia.org/wiki/E.164). Exemplo: `(86) 8370-9831` → `+558683709831` |
| `gender` | `male` → `M`, `female` → `F` |
| `dob.age`, `registered.age` | Remover estes campos |
| `nationality` | Adicionar campo com valor `BR` |
| `region` | Adicionar baseado no estado (Norte, Nordeste, Centro-Oeste, Sudeste, Sul) |

### 3. Contrato de Saída

```json
{
  "type": "laborious",
  "gender": "M",
  "name": {
    "title": "mr",
    "first": "quirilo",
    "last": "nascimento"
  },
  "location": {
    "region": "sul",
    "street": "680 rua treze",
    "city": "varginha",
    "state": "paraná",
    "postcode": 37260,
    "coordinates": {
      "latitude": "-46.9519",
      "longitude": "-57.4496"
    },
    "timezone": {
      "offset": "+8:00",
      "description": "Beijing, Perth, Singapore, Hong Kong"
    }
  },
  "email": "quirilo.nascimento@example.com",
  "birthday": "1979-01-22T03:35:31Z",
  "registered": "2005-07-01T13:52:48Z",
  "telephoneNumbers": ["+556629637520"],
  "mobileNumbers": ["+553270684089"],
  "picture": {
    "large": "https://randomuser.me/api/portraits/men/83.jpg",
    "medium": "https://randomuser.me/api/portraits/med/men/83.jpg",
    "thumbnail": "https://randomuser.me/api/portraits/thumb/men/83.jpg"
  },
  "nationality": "BR"
}
```

---

## Requisitos da API

### Endpoint

```
GET /users
```

### Parâmetros de Query

| Parâmetro | Tipo | Descrição |
|-----------|------|-----------|
| `region` | string | Filtrar por região (norte, nordeste, centro-oeste, sudeste, sul) |
| `type` | string | Filtrar por classificação (special, normal, laborious) |
| `pageNumber` | int | Número da página (começando em 1) |
| `pageSize` | int | Itens por página |

### Resposta

```json
{
  "pageNumber": 1,
  "pageSize": 10,
  "totalCount": 2000,
  "users": [...]
}
```

### Validação

Sua API deve passar no nosso script de validação:

```bash
./validate.sh
```

Isso verifica:
- Endpoint respondendo em `localhost:8080`
- Campos de paginação presentes
- Contagem total de 2000 registros

---

## Critérios de Avaliação

Avaliamos sua entrega em **7 competências**. Não há "níveis" para escolher — apenas entregue o seu melhor trabalho, e nós avaliaremos onde você se encaixa.

### 1. 🎯 Resolução de Problemas

| O que procuramos |
|------------------|
| Implementação correta de todas as regras de negócio |
| Tratamento de casos extremos (dados inválidos, campos faltantes, entrada malformada) |
| Abordagem lógica e eficiente para transformação de dados |

### 2. 🏗️ Arquitetura de Código

| O que procuramos |
|------------------|
| Clara separação de responsabilidades |
| Estrutura de projeto consistente |
| Uso apropriado de design patterns (quando agregam valor, não por vaidade) |
| Código fácil de navegar e entender |

### 3. ✨ Qualidade de Código

| O que procuramos |
|------------------|
| Legibilidade acima de esperteza |
| Convenções de nomenclatura significativas |
| Estilo consistente em todo o código |
| Sem complexidade desnecessária |
| Tratamento adequado de erros |

### 4. 🧪 Testes

| O que procuramos |
|------------------|
| Testes que documentam comportamento |
| Cobertura de caminhos críticos |
| Testes que pegariam bugs reais |
| Equilíbrio entre testes unitários e de integração |

### 5. 📚 Documentação

| O que procuramos |
|------------------|
| README claro com instruções de setup |
| Documentação da API (qualquer formato) |
| Comentários onde o código não é autoexplicativo |
| Decisões de arquitetura explicadas (quando relevante) |

### 6. 🚀 Prontidão para Produção

| O que procuramos |
|------------------|
| Containerização (Docker) |
| Configuração de ambiente |
| Health checks |
| Estratégia de logging |
| Conhecimento de CI/CD |

### 7. 🤖 Colaboração com IA

| O que procuramos |
|------------------|
| Transparência no uso de IA |
| Pensamento crítico sobre código gerado por IA |
| Iteração e refinamento ao invés de copiar e colar |
| Compreensão do que a IA produziu |

---

## Jornada IA (Obrigatório)

Crie uma pasta `/ai-journey` no seu repositório documentando como você usou ferramentas de IA.

### Arquivos Obrigatórios

```
📁 ai-journey/
├── README.md          # Resumo do seu uso de IA
├── prompts.md         # Principais prompts que você usou
└── learnings.md       # O que você aprendeu no processo
```

### O que Documentar

**prompts.md** — Não documente tudo, apenas as partes interessantes:

```markdown
## Prompt: Regex para telefone
**Ferramenta:** ChatGPT-4

**O que perguntei:**
"Crie uma regex para converter telefones brasileiros para formato E.164"

**O que aconteceu:**
Regex inicial não tratava celulares com 9 dígitos. Eu tive que...

**Solução final:**
[seu código]
```

**learnings.md** — Reflita sobre a experiência:

```markdown
## O que funcionou bem
- IA foi ótima para código boilerplate
- Me ajudou a explorar bibliotecas que não conhecia

## O que não funcionou
- Sugestão inicial de arquitetura era over-engineered
- Tive que simplificar depois de entender os requisitos reais

## O que faria diferente
- Começar com requisitos mais claros nos prompts
- Pedir soluções mais simples primeiro
```

---

## Entrega

### Linguagens

**Python** ou **C#** — escolha a que você tem mais conforto.

### Estrutura do Repositório

```
📁 seu-repo/
├── src/                  # Código fonte
├── tests/                # Testes
├── ai-journey/           # Documentação de IA (obrigatório)
├── docker-compose.yml    # Se aplicável
└── README.md             # Instruções de setup
```

### Como Entregar

1. Crie um repositório **público** no GitHub
2. Abra uma **Issue** neste repositório com:
   - Título: `[Backend] Seu Nome`
   - Link para seu repositório
   - Breve descrição da sua abordagem
   - Qualquer coisa que queira que saibamos

### Prazo

- **Recomendado:** 7 dias
- **Precisa de mais tempo?** Só nos avise na issue

---

## FAQ

<details>
<summary><b>Quais linguagens posso usar?</b></summary>

Python ou C#. Escolha aquela com a qual você se sente mais confortável.
</details>

<details>
<summary><b>Há vagas abertas?</b></summary>

Nem sempre, mas mantemos um banco de talentos. Boas entregas ficam no nosso radar para oportunidades futuras.
</details>

<details>
<summary><b>E se eu só conseguir completar parte do desafio?</b></summary>

Entregue o que você tem! Entregas parciais com código de qualidade nos dizem mais do que entregas completas com código ruim. Apenas documente o que está faltando e por quê.
</details>

<details>
<summary><b>Devo incluir funcionalidades extras?</b></summary>

Somente se agregarem valor claro e não comprometerem os requisitos principais. Preferimos o básico bem executado do que extras pela metade.
</details>

<details>
<summary><b>Como vou saber meu nível de senioridade?</b></summary>

Não pedimos que você declare um nível. Avaliamos sua entrega em todos os critérios e determinamos o fit baseado em nossos padrões internos.
</details>

---

## Outros Desafios

Se você está aplicando para uma vaga de front-end, confira nosso [frontend-challenge](https://github.com/juntossomosmais/frontend-challenge).

---

## Dúvidas?

Abra uma [issue](../../issues) ou entre em contato pelo **vagas-dev@juntossomosmais.com.br**.

Antes de perguntar, verifique se sua dúvida já foi respondida em [issues anteriores](../../issues?q=is%3Aissue).

---

<p align="center">
  <sub>Feito com 💛 pelo Time de Engenharia da <a href="https://juntossomosmais.com.br">Juntos Somos Mais</a></sub>
</p>

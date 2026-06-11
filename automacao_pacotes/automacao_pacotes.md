# Guia de Pacotes Essenciais para Ciência de Dados em R

## Introdução

Ao trabalhar com Ciência de Dados em R, é fundamental conhecer os principais pacotes utilizados para importação, limpeza, transformação e visualização de dados.

Este guia apresenta os pacotes recomendados, explica a diferença entre instalar e carregar pacotes, além de mostrar como automatizar o carregamento utilizando o arquivo `.Rprofile`.

---

# O Pacote `foreign`

## O que é?

O pacote `foreign` foi durante muitos anos uma das principais soluções para importar dados de outros softwares estatísticos para o R.

Instalação:

```r
install.packages("foreign")
library(foreign)
```

## Para que serve?

Permite importar arquivos provenientes de diversos softwares estatísticos.

| Formato      | Função           |
| ------------ | ---------------- |
| SPSS (.sav)  | `read.spss()`    |
| Stata (.dta) | `read.dta()`     |
| SAS          | `read.xport()`   |
| Minitab      | `read.mtp()`     |
| Systat       | `read.sysstat()` |

### Exemplo

```r
library(foreign)

dados <- read.spss(
  "pesquisa.sav",
  to.data.frame = TRUE
)
```

---

## O `foreign` serve para CSV?

**Não.**

Para arquivos CSV utilize pacotes mais modernos como:

* `readr`
* `data.table`

Funções recomendadas:

```r
read_csv()
read_csv2()
fread()
```

---

## Limitações do `foreign`

Embora ainda seja mantido pelo projeto R, apresenta algumas limitações:

* ❌ Mais lento
* ❌ Menor suporte a UTF-8
* ❌ Menor suporte a formatos modernos
* ❌ Menos recursos que soluções atuais

---

# O Substituto Moderno: `haven`

Atualmente a comunidade utiliza o pacote `haven`.

Instalação:

```r
install.packages("haven")
library(haven)
```

## Exemplos

### SPSS

```r
dados <- read_sav("arquivo.sav")
```

### Stata

```r
dados <- read_dta("arquivo.dta")
```

### SAS

```r
dados <- read_sas("arquivo.sas7bdat")
```

---

# Pacotes Recomendados para Ciência de Dados

Instale todos de uma vez:

```r
install.packages(c(
  "tidyverse",
  "readr",
  "data.table",
  "readxl",
  "haven",
  "rio",
  "janitor",
  "lubridate"
))
```

---

## Quando utilizar cada pacote?

| Necessidade            | Pacote       |
| ---------------------- | ------------ |
| CSV e CSV2             | `readr`      |
| Arquivos muito grandes | `data.table` |
| Excel                  | `readxl`     |
| SPSS / Stata / SAS     | `haven`      |
| Importação universal   | `rio`        |
| Limpeza de dados       | `janitor`    |
| Datas                  | `lubridate`  |
| Manipulação de dados   | `tidyverse`  |

---

# Instalar x Carregar Pacotes

Uma dúvida muito comum é:

> Preciso carregar os pacotes toda vez que abrir o R?

**Sim.**

---

## Instalar

Instala o pacote apenas uma vez no computador.

```r
install.packages("readr")
```

Analogia:

É como instalar o Microsoft Word.

---

## Carregar

Carrega o pacote para a sessão atual do R.

```r
library(readr)
```

Analogia:

É como abrir o Word.

Toda vez que iniciar uma nova sessão do R, será necessário carregar novamente os pacotes.

---

# Fluxo Normal de Trabalho

Primeira vez:

```r
install.packages("tidyverse")
```

Nos próximos projetos:

```r
library(tidyverse)
```

---

# Como os Profissionais Fazem

Normalmente os pacotes ficam declarados no início de cada script.

```r
# Pacotes
library(tidyverse)
library(readxl)
library(janitor)
library(lubridate)
```

Isso facilita a manutenção do código e a colaboração com outras pessoas.

---

# Automatizando com `.Rprofile`

O arquivo `.Rprofile` é executado automaticamente sempre que o R ou o RStudio é iniciado.

---

## Criando pelo RStudio

No Console:

```r
file.edit("~/.Rprofile")
```

Será aberto um arquivo vazio.

Adicione:

```r
library(tidyverse)
library(readxl)
library(janitor)
library(lubridate)
library(DT)
```

Salve o arquivo.

Feche e abra o RStudio novamente.

---

## Criando Manualmente no Windows

Normalmente o símbolo `~` aponta para:

```text
C:\Users\SEU_USUARIO\
```

Crie um arquivo chamado:

```text
.Rprofile
```

### Atenção

Correto:

```text
.Rprofile
```

Incorreto:

```text
Rprofile.txt
```

```text
.Rprofile.txt
```

---

# Verificando se Funcionou

Após reiniciar o RStudio:

```r
search()
```

Resultado esperado:

```r
[1] ".GlobalEnv"
[2] "package:DT"
[3] "package:lubridate"
[4] "package:janitor"
[5] "package:readxl"
[6] "package:tidyverse"
```

---

# Versão Profissional do `.Rprofile`

Evita erros caso algum pacote não esteja instalado.

```r
if (requireNamespace("tidyverse", quietly = TRUE))
  library(tidyverse)

if (requireNamespace("readxl", quietly = TRUE))
  library(readxl)

if (requireNamespace("janitor", quietly = TRUE))
  library(janitor)

if (requireNamespace("lubridate", quietly = TRUE))
  library(lubridate)

if (requireNamespace("DT", quietly = TRUE))
  library(DT)
```

---

# Modelo de `.Rprofile` para Ciência de Dados

```r
options(stringsAsFactors = FALSE)

library(tidyverse)
library(readxl)
library(writexl)
library(janitor)
library(lubridate)
library(DT)
library(data.table)
library(haven)
```

---

# Benefícios dessa Configuração

Ao abrir o RStudio você já terá disponível:

* ✅ CSV (`read_csv`, `read_csv2`)
* ✅ Excel (`read_excel`)
* ✅ SPSS e Stata (`haven`)
* ✅ Tabelas interativas (`DT`)
* ✅ Limpeza de dados (`janitor`)
* ✅ Datas (`lubridate`)
* ✅ Arquivos grandes (`data.table`)
* ✅ Todo o ecossistema `tidyverse`

---

# Conclusão

Para projetos modernos de Ciência de Dados:

* Prefira `haven` ao invés de `foreign`.
* Utilize `readr` para arquivos CSV.
* Utilize `data.table` para grandes volumes de dados.
* Utilize `.Rprofile` para automatizar o ambiente de trabalho.
* Mantenha os pacotes explicitamente declarados nos scripts para facilitar a manutenção e colaboração.

Seguindo essas práticas, você terá um ambiente robusto, moderno e alinhado com o mercado para desenvolvimento de projetos em Ciência de Dados com R.

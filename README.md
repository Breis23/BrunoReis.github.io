# BrunoReis.github.io
Projeto baseado no Google Data Analytics Capstone, analisando dados da Cyclistic, empresa fictícia de bicicletas compartilhadas em Chicago. O objetivo é identificar diferenças no uso entre ciclistas casuais e membros anuais, gerando insights estratégicos para o marketing focar na conversão e fidelização de usuários.
# 🚴‍♂️ Cyclistic Bike-Share Case Study  

**Autor:** Bruno Eduardo Souza Reis  
**Linguagem:** R  
**Ferramentas:** RStudio, tidyverse, lubridate, ggplot2, patchwork  
**Período Analisado:** 1º trimestre de 2019 e 1º trimestre de 2020  

---

## 🏙️ Contexto do Estudo  

Este projeto faz parte do estudo de caso **Google Data Analytics Capstone**.  
A empresa fictícia **Cyclistic**, uma empresa de compartilhamento de bicicletas em Chicago, deseja entender **como ciclistas casuais e membros anuais usam as bicicletas de forma diferente**.  

O objetivo é obter **insights estratégicos** que ajudem o time de marketing a **converter ciclistas casuais em membros anuais**, aumentando a receita e a fidelização dos clientes.  

---

## 🔍 Pergunta de Negócio  

> **Como os membros anuais e os ciclistas casuais usam as bicicletas Cyclistic de forma diferente?**

---

## 🧮 Etapas do Projeto  

### **1. Prepare & Process**  

- Importação dos datasets `Divvy_2019_Q1.csv` e `Divvy_2020_Q1.csv`.  
- Padronização das colunas para manter consistência entre os arquivos.  
- Criação de colunas analíticas principais:
  - `ride_length` → duração em minutos  
  - `day_of_week` → dia da semana da viagem  
- Remoção de registros inválidos (viagens com duração ≤ 0 ou dados incompletos).  

```r
library(tidyverse)
library(lubridate)

# Importar e unir os dados
df_2019_q1 <- read_csv("data/Divvy_2019_Q1.csv")
df_2020_q1 <- read_csv("data/Divvy_2020_Q1.csv")

df_unido <- bind_rows(df_2019_q1, df_2020_q1)

# Criar colunas analíticas
df_clean <- df_unido %>%
  mutate(
    ride_length = as.numeric(difftime(ended_at, started_at, units = "mins")),
    day_of_week = wday(started_at, label = TRUE, abbr = FALSE)
  ) %>%
  filter(ride_length > 0)

# Analyze - Estatísticas gerais por tipo de usuário
estatisticas_gerais <- df_clean %>%
  group_by(member_casual) %>%
  summarise(
    total_viagens = n(),
    media_duracao = mean(ride_length),
    mediana_duracao = median(ride_length),
    max_duracao = max(ride_length),
    min_duracao = min(ride_length)
  )

# Estatísticas por dia da semana
df_analise <- df_clean %>%
  group_by(member_casual, day_of_week) %>%
  summarise(
    numero_viagens = n(),
    duracao_media = mean(ride_length)
  )
write_csv(df_analise, "outputs/cyclistic_sumario_por_dia_e_membro.csv")


#Visualizações - Volume de viagens por dia da semana
df_analise %>%
  ggplot(aes(x = day_of_week, y = numero_viagens, fill = member_casual)) +
  geom_col(position = "dodge") +
  labs(
    title = "Volume Total de Viagens: Membros Anuais vs. Casuais",
    subtitle = "Casuais dominam fins de semana, Membros dominam dias úteis",
    x = "Dia da Semana",
    y = "Total de Viagens",
    fill = "Tipo de Ciclista"
  ) +
  theme_minimal()

# Duração Média das Viagens
df_analise %>%
  ggplot(aes(x = day_of_week, y = duracao_media, fill = member_casual)) +
  geom_col(position = "dodge") +
  labs(
    title = "Duração Média dos Trajetos (Minutos)",
    subtitle = "Casuais usam as bicicletas por mais tempo",
    x = "Dia da Semana",
    y = "Duração Média (min)",
    fill = "Tipo de Ciclista"
  ) +
  theme_minimal()

# Reprodutibilidade - Para reproduzir a análise
git clone https://github.com/bruno-reis-data/Cyclistic_Case_Study.git
cd Cyclistic_Case_Study

# No RStudio
source("scripts/01_prepare_process.R")
source("scripts/02_analyze.R")
source("scripts/03_visualize.R")

# Os resultados serão gerados em
outputs/cyclistic_sumario_por_dia_e_membro.csv
outputs/plots/

# Principais Bibliotecas
library(tidyverse)
library(lubridate)
library(ggplot2)
library(patchwork)

# Estrutura do Projeto
Cyclistic_Case_Study/
│
├── data/
│   ├── Divvy_2019_Q1.csv
│   ├── Divvy_2020_Q1.csv
│
├── scripts/
│   ├── 01_prepare_process.R
│   ├── 02_analyze.R
│   ├── 03_visualize.R
│
├── outputs/
│   ├── cyclistic_sumario_por_dia_e_membro.csv
│   ├── plots/
│
├── progresso_cyclistic.RData
│
└── README.md

#Conclusão
O estudo revela que os membros anuais e os ciclistas casuais têm padrões de uso claramente distintos:

Casuais valorizam lazer e experiências de fim de semana.

Membros usam o sistema como meio de transporte diário.

Esses achados sustentam ações estratégicas que direcionam campanhas personalizadas e maximizam conversão e retenção de clientes.

# Este projeto foi desenvolvido como parte do portfólio analítico de Bruno Eduardo Souza Reis.

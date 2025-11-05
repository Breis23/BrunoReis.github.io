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

# 🚜 Dashboard de Análise de Material Rodante

> Ferramenta analítica para monitoramento de desgaste de esteiras de perfuratrizes — **Vale Base Metals**

![Status](https://img.shields.io/badge/status-em%20desenvolvimento-blue)
![Tecnologia](https://img.shields.io/badge/stack-HTML%20%2F%20CSS%20%2F%20JavaScript-29ABE2)
![Frota](https://img.shields.io/badge/frota-Pit%20Viper%20DR416i%20%7C%20FlexiROC%20D65-lightgrey)

---

## 📋 Sobre o Projeto

Este dashboard foi desenvolvido para apoiar analistas técnicos na gestão do desgaste de componentes de material rodante (esteiras) de uma frota de perfuratrizes de grande porte. A ferramenta transforma dados brutos de inspeção em insights acionáveis, permitindo priorização de manutenção preventiva e redução de paradas não planejadas.

**Frota coberta:** 16 equipamentos — Pit Viper DR416i e FlexiROC D65  
**Período de dados:** Agosto/2025 a Fevereiro/2026  
**Componentes monitorados:** Roda Guia, Elo, Motriz, Roletes Superior/Inferior, Sapata, Bucha, Passo da Esteira

---

## 🖥️ Funcionalidades

### 5 abas analíticas:

**1. Visão Geral**
- KPIs da frota: equipamentos críticos, em atenção e normais
- Desgaste médio por componente (gráfico de barras)
- Roletes danificados por equipamento (última inspeção, LC + LNC)
- Cards individuais por equipamento com mini-barras de desgaste

**2. Heatmap de Desgaste**
- Tabela colorida com % de desgaste de cada componente por equipamento
- Escala visual: Normal → Atenção → Crítico → Ultrapassou limite

**3. Alertas & Críticos**
- Lista de componentes acima de 80% do limite de desgaste
- Tabela de roletes danificados por equipamento com nível de urgência

**4. Taxa de Desgaste**
- Ranking dos componentes com maior taxa de desgaste (%/1000h)
- Gráfico de evolução temporal da Roda Guia nos equipamentos críticos
- Cards interpretativos com diagnósticos por equipamento

**5. Assimetria LC/LNC**
- Comparação do desgaste entre Lado Cabine e Lado Não-Cabine
- Identificação de desgaste desigual (indicador de tensionamento irregular, operação em curvas ou piso inclinado)

---

## 📊 Lógica de Cálculo

### % de Desgaste
```
Componentes decrescentes (ELO, MOTRIZ, SAPATA, BUCHA, ROLETE INFERIOR):
  % desgaste = (Valor_Novo - Medição) / (Valor_Novo - Limite) × 100

Componentes crescentes (PASSO DA ESTEIRA, RODA GUIA):
  % desgaste = (Medição - Valor_Novo) / (Limite - Valor_Novo) × 100
```

### Taxa de Desgaste
```
Taxa (%/1000h) = Δ% desgaste / Δ horímetro × 1000
```
Calculada entre a primeira e a última medição disponível por equipamento/lado.

### Roletes Danificados (frota)
```
Total = Σ (LC + LNC) da última inspeção de cada TAG
```
Considera apenas a inspeção mais recente de cada equipamento, somando ambos os lados.

### Assimetria
```
Diferença = |% desgaste LC - % desgaste LNC|
Alerta acionado quando diferença > 10 pontos percentuais
```

---

## 🎨 Design

O dashboard segue a identidade visual da **Vale Base Metals**:

| Elemento | Cor | Uso |
|---|---|---|
| Header / primária | `#29ABE2` | Fundo do cabeçalho, destaque principal |
| Navegação | `#1a87b8` | Barra de abas |
| Fundo | `#07192a` | Background dark navy |
| Alerta crítico | `#e05252` | Desgaste > 80% |
| Atenção | `#f0b429` | Desgaste 50–80% |
| Normal | `#6dcbee` | Desgaste < 25% |

**Tipografia:** IBM Plex Sans (corpo) + IBM Plex Mono (valores e métricas)

---

## 📁 Estrutura do Projeto

```
/
├── dashboard_material_rodante.html   # Dashboard completo (single-file)
├── README.md                          # Este arquivo
└── dados/
    ├── Ger__de_Material_Rodante_GD.csv   # Dados de medição das inspeções
    └── Parâmetros.csv                     # Limites de desgaste por componente/modelo
```

---

## 📂 Formato dos Dados de Entrada

### `Ger__de_Material_Rodante_GD.csv`
| Campo | Descrição |
|---|---|
| TAG | Identificador do equipamento |
| MODELO | Pit Viper/DR416i ou FlexiROC D65 |
| OM | Número da ordem de manutenção |
| DATA | Data da inspeção (MM/DD/YYYY) |
| HORÍMETRO | Horas de operação do equipamento |
| LADO | LC (lado cabine) ou LNC (lado não cabine) |
| RODA GUIA, ELO, MOTRIZ... | Medições em mm de cada componente |
| ROLETES DANIFICADOS (VISUAL) | Contagem visual de roletes danificados |

### `Parâmetros.csv`
| Campo | Descrição |
|---|---|
| Componente | Nome do componente |
| Modelo | Modelo do equipamento |
| Novo | Medição de referência (componente novo) |
| Limite de desgaste | Medição limite antes da substituição |

---

## 🚀 Como Usar

### Visualização local
```bash
# Basta abrir o arquivo no navegador
open dashboard_material_rodante.html
# ou
xdg-open dashboard_material_rodante.html
```

### Deploy gratuito

**Opção 1 — GitHub Pages**
1. Faça o commit do `dashboard_material_rodante.html` na raiz ou pasta `/docs`
2. Acesse: Settings → Pages → Source: branch `main`, pasta `/root` ou `/docs`
3. Acesse via `https://<seu-usuario>.github.io/<repositorio>`

**Opção 2 — Netlify Drop**
1. Acesse [netlify.com/drop](https://netlify.com/drop)
2. Arraste o arquivo `dashboard_material_rodante.html`
3. URL pública gerada automaticamente em segundos

**Opção 3 — Vercel**
```bash
npm i -g vercel
vercel deploy
```

---

## 🔮 Próximos Passos

- [ ] Integração com Google Sheets para atualização automática dos dados (sem editar o HTML)
- [ ] Filtros interativos por modelo, área de inspeção e período
- [ ] Cálculo de vida útil residual estimada por componente
- [ ] Exportação de relatório PDF por equipamento
- [ ] Suporte a dados do FlexiROC D65 nas abas de desgaste (atualmente Pit Viper only)
- [ ] Alertas por e-mail quando componente ultrapassar limiar crítico

---

## 👤 Autoria

Desenvolvido pela equipe de **Gestão de Material Rodante — Vale Base Metals**  
Analista responsável pelas inspeções: **Bruno Silva**

---

## 📄 Licença

Uso interno — Vale Base Metals. Não distribuir externamente.

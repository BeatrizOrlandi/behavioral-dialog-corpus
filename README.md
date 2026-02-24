# Resultados — Geração e Avaliação de Diálogos Corporativos Sintéticos

Este repositório contém os artefatos gerados pelo pipeline de pesquisa da monografia sobre geração controlada de diálogos corporativos multi-turno condicionados a variáveis latentes ocupacionais.

---

## Estrutura do Repositório

```
.
├── analysis/                    # Métricas, tabelas e visualizações
├── dialogs/                     # Diálogos gerados por cenário/provedor
├── teams/                       # Dossiês de planejamento por time
├── scenario_datasets/           # Datasets de cenário (variáveis latentes)
├── trajectory_groups/           # Agrupamentos de trajetória profissional
├── manifests/                   # Manifestos de seleção de times
├── _batches/                    # Controle de execução batch
└── _logs/                       # Logs de auditoria
```

---

## `analysis/`

Resultados consolidados da análise do corpus.

### Tabelas de Métricas

| Arquivo | Descrição |
|---------|-----------|
| `tab01_global_metrics.csv` | Métricas globais por dimensão (accuracy, F1, MCC, kappa) |
| `tab01b_perclass_metrics.csv` | Métricas por classe (LOW/MID/HIGH) |
| `tab03_metrics_by_scenario.csv` | Comparação entre cenários |
| `tab04_metrics_by_inferrer.csv` | Métricas por provedor inferidor |
| `tab05_metrics_by_generator.csv` | Métricas por provedor gerador |
| `tab06_cross_metrics.csv` | Cruzamento gerador × inferidor |
| `tab08_self_vs_cross.csv` | Self-evaluation vs cross-evaluation |
| `tab10_mcnemar.csv` | Testes McNemar pareados |
| `tab11_cochran_q.csv` | Testes Cochran's Q |
| `tab12_bootstrap_ci.csv` | Intervalos de confiança bootstrap 95% |
| `tab15_by_affinity_band.csv` | Métricas por banda de afinidade |
| `tab16_mid_bias.csv` | Análise de viés de predição MID |
| `tab17_summary_full.csv` | Tabela resumo completa |
| `tab17b_summary_compact.csv` | Tabela resumo compacta |

### Tabelas de Validação (P3)

| Arquivo | Descrição |
|---------|-----------|
| `tab_v1_validation_rate.csv` | Taxa de validação por gerador |
| `tab_v2_issues_by_code.csv` | Issues por código |
| `tab_v2b_issues_by_severity.csv` | Issues por severidade |
| `tab_v3_issues_by_generator.csv` | Issues por gerador |
| `tab_v3b_issue_codes_by_generator.csv` | Códigos de issue por gerador |
| `tab_v4_rubrics_detailed.csv` | Rubricas detalhadas |
| `tab_v5_validation_vs_accuracy.csv` | Validação vs acurácia |
| `tab_v6_correlation_matrix.csv` | Matriz de correlação |

### Tabelas de Qualidade

| Arquivo | Descrição |
|---------|-----------|
| `tab13_rubric_descriptive.csv` | Estatísticas descritivas das rubricas |
| `tab14_rubric_correlation.csv` | Correlação rubricas × acurácia |
| `tab_extra_class_distribution.csv` | Distribuição de classes |

### Dados Brutos

| Arquivo | Descrição |
|---------|-----------|
| `raw_dataframe.csv` | DataFrame completo de inferências |
| `raw_validation.csv` | Dados brutos de validação |
| `raw_issues.csv` | Issues identificados |
| `corpus_summary.json` | Sumário estatístico do corpus |

### Visualizações

| Pasta/Arquivo | Descrição |
|---------------|-----------|
| `confusion_by_inferrer/` | Matrizes de confusão por inferidor |
| `confusion_by_scenario/` | Matrizes de confusão por cenário |
| `confusion_cross/` | Matrizes de confusão cruzadas (gerador → inferidor) |
| `fig_extra_barplot_gen_inf.png` | Barplot comparativo gerador × inferidor |

---

## `dialogs/`

Diálogos gerados organizados por cenário, replicação, provedor e modelo.

```
dialogs/
├── S_full_high_noise/           # Cenário com latentes estruturadas + alto ruído
│   └── rep_11/                  # Replicação representativa
│       ├── gen_claude/claude-haiku-4-5/
│       ├── gen_gemini/gemini-2.5-pro/
│       └── gen_openai/gpt-5.1/
│
└── S_random_with_latents/       # Cenário baseline (latentes aleatórias)
    └── rep_12/                  # Replicação representativa
        ├── gen_claude/claude-haiku-4-5/
        ├── gen_gemini/gemini-2.5-pro/
        └── gen_openai/gpt-5.1/
```

### Arquivos por Time (em `team_{BAND}_{ID}/generation/`)

| Arquivo | Descrição |
|---------|-----------|
| `channel.json` | Diálogo gerado (canal Slack multi-thread) |
| `ground_truth_latent.json` | Labels verdadeiras do cenário |
| `validation_prompt3_gemini.json` | Resultado da validação P3 |
| `inverse_inference_gemini.json` | Inferência inversa pelo Gemini |
| `inverse_inference_claude.json` | Inferência inversa pelo Claude (self-eval) |
| `inverse_inference_openai.json` | Inferência inversa pelo OpenAI (self-eval) |

---

## `teams/`

Dossiês de planejamento (Prompt 1) por time, independentes de cenário.

```
teams/
├── HIGH_00186/                  # Time com alta afinidade de trajetória
├── MID_00280/                   # Time com afinidade moderada
├── LOW_03413/                   # Time com baixa afinidade
└── ...
```

### Arquivos por Time (em `{BAND}_{ID}/planning/`)

| Arquivo | Descrição |
|---------|-----------|
| `dossier.json` | PROJECT_PACK + BLUEPRINT estruturado |
| `raw_llm_output.json` | Saída bruta do LLM planner |

### Distribuição de Times por Banda

| Banda | Quantidade |
|-------|------------|
| HIGH | 62 |
| MID | 40 |
| LOW | 3 |

---

## `scenario_datasets/`

Datasets de cenário com variáveis latentes simuladas.

```
scenario_datasets/
├── manifest_representatives.csv     # Replicações representativas
├── S_full_high_noise/
│   ├── per_id.csv                   # Agregação por pessoa
│   ├── per_id.parquet
│   ├── per_experience.parquet       # Por experiência profissional
│   └── chosen_rep.txt               # Replicação escolhida
│
└── S_random_with_latents/
    └── ...
```

### Cenários

| Cenário | Descrição | Replicação |
|---------|-----------|------------|
| `S_full_high_noise` | Latentes estruturadas com alto ruído | rep_11 |
| `S_random_with_latents` | Latentes aleatórias (baseline) | rep_12 |

---

## `trajectory_groups/`

Agrupamentos de trajetória profissional usados para formação de times.

| Arquivo | Descrição |
|---------|-----------|
| `groups_summary.csv` | Resumo dos grupos (id, tamanho, categoria) |
| `group_members.csv` | Membros de cada grupo (_id, group_id) |
| `per_id_trajectory.parquet` | Trajetórias por pessoa |
| `traj.duckdb` | Base DuckDB de trajetórias |
| `report_discarded_ids.csv` | IDs descartados |
| `report_group_size_distribution.csv` | Distribuição de tamanho dos grupos |
| `report_similarity_summary.csv` | Sumário de similaridade |

---

## `manifests/`

| Arquivo | Descrição |
|---------|-----------|
| `selected_teams_105.json` | Manifesto dos 105 times selecionados |

---

## `_batches/`

Controle de execução batch do pipeline.

```
_batches/
├── pending/     # Jobs JSONL pendentes
└── results/     # Resultados JSONL com checkpoint
```

---

## `_logs/`

Logs de auditoria do pipeline.

| Arquivo | Descrição |
|---------|-----------|
| `llm_jobs.jsonl` | Registro de todas as chamadas LLM |
| `roles_audit.csv` | Auditoria de atribuição de papéis |

---

## Modelos Utilizados

| Provedor | Modelo | Papel |
|----------|--------|-------|
| OpenAI | GPT-5.1 | Planner (P1), Gerador (P2), Inferidor (P4 self-eval) |
| Google | Gemini 2.5 Pro | Gerador (P2), Validador (P3), Inferidor (P4 universal) |
| Anthropic | Claude Haiku 4.5 | Gerador (P2), Inferidor (P4 self-eval) |

---

## Referência

Este repositório é parte da monografia:

> **Geração Sintética de Diálogos Profissionais usando Modelos de Linguagem e com a Incorporação de Histórico Curricular e Indicadores de Performance**
> 
> Pipeline de pesquisa para geração controlada de diálogos corporativos multi-turno estilo Slack, condicionados a variáveis latentes ocupacionais (desempenho, produtividade e habilidade), com avaliação em três níveis e inferência inversa multi-provedor.

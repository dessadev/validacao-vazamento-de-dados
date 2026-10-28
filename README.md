# Validação honesta: vazamento de dados, out of time e amostra pequena

Quatro experimentos controlados que mostram como um modelo pode **parecer melhor do que é**, e como evitar cada armadilha.

> ⚠️ Todos os dados são **sintéticos**, gerados no próprio notebook. Nenhum dado real é usado.

## Por que este projeto

Um resultado perfeito também pede desconfiança. Um acerto de 100% em poucos casos pode esconder vazamento de dados, que só aparece quando a base é ampliada. Este notebook reproduz esse tipo de armadilha com dados controlados.

## Resultados

| # | Armadilha | Resultado enganoso | Resultado honesto |
|---|---|---|---|
| 1 | Variável do futuro (`estorno_registrado`) | AUC **0,970** | AUC **0,701** |
| 2 | Seleção de variáveis antes do split, em dado de **puro ruído** | AUC **0,783** | AUC **0,512** (chute) |
| 3 | Split aleatório com mudança de padrão no tempo | AUC **0,617** | AUC out of time **0,354** |
| 4 | Amostra pequena | 10 de 10 = 100% | IC 95% de **69% a 100%** |

No experimento 4, o mesmo cálculo para 92 acertos em 100 casos dá um intervalo de 84,8% a 96,5%, e com 460 acertos em 500 casos o intervalo fecha em 89,3% a 94,2%.

## Como evitar

| Armadilha | Sinal de alerta | Prevenção |
|---|---|---|
| Variável do futuro | uma variável explica quase tudo | perguntar se o valor existe no momento da decisão |
| Pré-processamento antes do split | resultado bom em dado sem sinal | todo o preparo dentro de um `Pipeline` |
| Split aleatório em dado temporal | queda de desempenho em produção | validação out of time e monitoramento |
| Amostra pequena | 100% com poucos casos | intervalo de confiança e gabarito maior |

## Conceitos

- **Overfitting:** o modelo decora o treino e vai mal em dado novo.
- **Vazamento de dados:** o modelo enxerga uma informação que não teria na vida real. Os dois inflam o resultado, mas por causas diferentes.
- **Concept drift:** a relação entre as variáveis e o alvo muda com o tempo, como quando o fraudador muda de tática.

## Como rodar

```bash
pip install -r requirements.txt
jupyter notebook validacao_vazamento.ipynb
```

---
Feito por [Andressa Prudente](https://github.com/dessadev)

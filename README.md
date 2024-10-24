# Análise de Salários na Área de Dados: Insights e Tendências

## Introdução
Com o crescente interesse na área de Ciência de Dados, muitos profissionais estão buscando entender as tendências salariais nesse campo para tomar decisões de carreira mais informadas. Neste artigo, exploraremos um conjunto de dados disponível no Kaggle que contém informações sobre salários de profissionais da área de dados ao redor do mundo. A análise ajudará a responder perguntas como:

1 - Qual o impacto do nível de experiência nos salários?
2 - Existem diferenças significativas de salário dependendo do local de trabalho ou configuração de trabalho?
3 - Quais fatores parecem influenciar mais o salário?

O artigo também é um guia passo a passo para quem deseja aprender mais sobre como realizar uma análise exploratória de dados e usar Python para obter insights acionáveis.

# Metodologia

## Ferramentas Utilizadas
Para esta análise, usamos as seguintes bibliotecas do Python:

* pandas para manipulação de dados
* matplotlib e seaborn para visualização de dados
* numpy para cálculos estatísticos
  
## Descrição dos Dados
O conjunto de dados utilizado contém as seguintes colunas:

* `work_year`: Ano de trabalho
* `job_title`: Cargo do profissional
* `job_category`: Categoria do cargo
* `salary_currency`: Moeda do salário
* `salary`: Salário na moeda original
* `salary_in_usd`: Salário convertido para USD
* `employee_residence`: Local de residência do empregado
* `experience_level`: Nível de experiência (Júnior, Pleno, Sênior, etc.)
* `employment_type`: Tipo de emprego (tempo integral, freelance, etc.)
* `work_setting`: Configuração de trabalho (remoto, híbrido, presencial)
* `company_location`: Localização da empresa
* `company_size`: Tamanho da empresa (pequena, média, grande)

Os dados foram verificados para garantir que não havia valores faltantes e que todos estavam no formato apropriado para análise.

# Análise Exploratória de Dados (EDA)

## 1. Distribuição de Salários
O primeiro passo foi observar a distribuição dos salários para entender o alcance e identificar possíveis discrepâncias.

```python
# Estatísticas descritivas de salário em USD
print(df['salary_in_usd'].describe())

# Histograma para visualizar a distribuição de salários
plt.figure(figsize=(10, 6))
sns.histplot(df['salary_in_usd'], bins=30, kde=True)
plt.title('Distribuição de Salários em USD')
plt.xlabel('Salário (USD)')
plt.ylabel('Frequência')
plt.show()
```

**Observações:** A distribuição dos salários mostrou uma grande variação, com algumas profissões recebendo significativamente mais do que outras. Também foram identificados possíveis outliers no topo da distribuição.

## 2. Impacto do Nível de Experiência nos Salários
Analisamos como diferentes níveis de experiência afetam os salários médios.

```python
# Gráfico de barras para salário médio por nível de experiência
plt.figure(figsize=(8, 5))
sns.barplot(data=df, x='experience_level', y='salary_in_usd', estimator=lambda x: x.mean(), ci=None, palette='viridis')
plt.title('Salário Médio por Nível de Experiência')
plt.xlabel('Nível de Experiência')
plt.ylabel('Salário Médio (USD)')
plt.show()
```
**Conclusão:** Como esperado, profissionais com mais experiência tendem a ter salários mais altos, com um aumento substancial ao passar do nível Júnior para Sênior.

## 3. Comparação de Salários por Localização
Foi realizada uma análise para identificar as diferenças salariais com base na localização da empresa e na residência dos empregados.

```python
# Salário médio por localização da empresa
company_salary = df.groupby('company_location')['salary_in_usd'].mean().sort_values(ascending=False).head(10)
sns.barplot(x=company_salary.values, y=company_salary.index, palette='coolwarm')
plt.title('Salário Médio por Localização da Empresa (Top 10)')
plt.xlabel('Salário Médio (USD)')
plt.ylabel('Localização da Empresa')
plt.show()
```
**Insights:** Empresas localizadas em países com maior custo de vida ou economias fortes, como os EUA e a Suíça, geralmente pagam mais.

## 4. Análises Mais Profundas: Médias e Medianas Salariais
Além das médias, também verificamos as medianas para entender melhor a distribuição e evitar que valores extremos influenciassem os resultados.

```python
# Comparar médias e medianas salariais por configuração de trabalho
work_setting_stats = df.groupby('work_setting')['salary_in_usd'].agg(['mean', 'median']).reset_index()
sns.barplot(data=work_setting_stats.melt(id_vars='work_setting'), x='work_setting', y='value', hue='variable', palette='Set3')
plt.title('Comparação de Média e Mediana Salarial por Configuração de Trabalho')
plt.xlabel('Configuração de Trabalho')
plt.ylabel('Salário (USD)')
plt.legend(title='Estatística')
plt.show()
```
**Resultados:** Os salários médios e medianos são ligeiramente mais altos para configurações de trabalho híbridas e remotas, indicando uma tendência positiva para trabalho flexível.

## 5. Análise de Correlação
Para identificar fatores que mais influenciam o salário, calculamos a correlação entre variáveis.

```python
# Calcular correlação entre variáveis numéricas
df_encoded = pd.get_dummies(df, columns=['experience_level', 'employment_type', 'work_setting', 'company_size', 'job_category'], drop_first=True)
correlation_matrix = df_encoded.corr()
salary_correlation = correlation_matrix['salary_in_usd'].sort_values(ascending=False)

print("Principais correlações positivas com o salário:")
print(salary_correlation.head(10))
```

**Principais Fatores Correlacionados:**  Os fatores com maior correlação positiva incluem o nível de experiência sênior e certas categorias de empregos técnicos especializados, como engenheiros de machine learning.


# Conclusão
Esta análise demonstrou que existem várias dimensões que influenciam significativamente o salário de profissionais na área de dados. Os fatores mais importantes incluem nível de experiência, localização da empresa e configuração de trabalho. O mercado para esses profissionais é variado e oferece muitas oportunidades, especialmente para quem busca flexibilidade no trabalho remoto.

Para quem deseja explorar mais, o código completo está disponível no repositório do GitHub para facilitar a replicação e expansão desta análise. Sinta-se à vontade para contribuir e compartilhar suas próprias descobertas!

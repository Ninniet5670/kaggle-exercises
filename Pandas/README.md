Arquivos CSV são chamados assim pois significam "Comma-Separated Values"



Para checar o tamanho de um DataFrame, usa-se o atributo `.shape`, sendo o primeiro valor a quantidade de itens e o segundo valor a quantidade de colunas



É possível usar o nome das colunas como atributo para selecionar uma Series de um DataFrame

caso seja um dicionário, pode-se usar também pela indexação

```python
book.title

# ou

book['title']
```

a desvantagem dos atributos é que nomes conjuntos com um espaço não podem ser lidos, como em `reviews.country providence`



Para selecionar a primeira linha, basta:

```python
book.iloc[0]
```

e para selecionar múltiplas linhas, bote mais uma lista dentro do `iloc`

É possível também acessar colunas por meio de ambos os métodos por meio de seu segundo parâmetro, a coluna, sendo o primeiro a linha



```
book.iloc[:, 0]
```

o código acima pega a primeira coluna do DataFrame

o ";" significa tudo quando combinado com outros seletores, indicando um range de valores

> entender o conceito de list slicing é muito bom pra isso



> This is particularly confusing when the DataFrame index is a simple numerical list, e.g. `0,...,1000`. In this case `df.iloc[0:1000]` will return 1000 entries, while `df.loc[0:1000]` return 1001 of them! To get 1000 elements using `loc`, you will need to go one lower and ask for `df.iloc[0:999]`.



```python
cols_idx = [0, 11]
df = reviews.iloc[:100, cols_idx] # CERTO

# df = reviews.iloc[:100, ['country', 'variety']] ERRADO
```

caso usado o fatiamento de listas, a indexação dentro do loc e iloc deverá ser alocada em uma variável, senão ocorrerá um erro



É possível também implementar condicionais para mostrar somente sobre um objeto

```python
italian_wines = reviews[reviews.country == 'Italy']
```

 o código acima percorre a parte "country" do DataFrame "reviews "e retorna outro DataFrame com todas as informações adicionais caso o país seja "Italy"



Para selecionar dados cujo valor estejam dentro de uma lista de valores, usa-se `.isin()`

```python
reviews[(reviews.country.isin(['Australia', 'New Zealand'])) & (reviews.points >= 95)]
```

o código acima retornará todos as vezes que os países "Australia" e "New Zealand" marcaram mais de 94 pontos



```python
reviews['index_backwards'] = range(len(reviews), 0, -1)

>>> 0         129971
1         129970
           ...  
129969         2
129970         1
Name: index_backwards, Length: 129971, dtype: int64
```

o código acima irá atribuir o número do ultimo valor ao primeiro e assim sucessivamente



Para calcular a mediana de um vetor, usa-se `.median()`

 

Para retornar o index do maior ou menor valor do vetor, usa-se `.idxmax()` e `.idxmin()` respectivamente



```python
# centered_price = reviews.price.map(lambda x: x - reviews.price.mean()) ERRADO

reviews_mean = reviews.price.mean()
centered_price = reviews.price.map(lambda x: x - reviews_mean) #CERTO

# centered_price = reviews.price - reviews.price.mean() MAIS CERTO AINDA
```

Deve-se alocar em uma variável o vetor para que rode adequadamente na função `map()`



```python
n_tropical = reviews.description.map(lambda phrase: 'tropical' in phrase).sum()
n_fruity = reviews.description.map(lambda phrase: 'fruity' in phrase).sum()

descriptor_counts = pd.Series([n_tropical, n_fruity], index=['tropical', 'fruity'])
```

o código acima irá contar em cada pedaço da coluna "description" se há as palavras "tropical" e "fruity", depois disso, irá criar uma Series com a soma das ocorrências de cada uma



Para usar condicionais equivalentes à `elif` em um lambda, basta deixar um `if else` dentro do outro:

é obrigatório botar um `else` caso sua função lambda tenha um `if`

```python
lambda x: 1 if (x >= 95) else (2 if x >= 85 else 1)
```



```python
def stars(row):
    if row.country == 'Canada' or row.points >= 95:
        return 3
    elif row.points >= 85:
        return 2
    else:
        return 1

star_ratings = reviews.apply(stars, axis=1)
```

o código acima utiliza-se do .apply() para checar cada linha do "points" se o valor é maior ou menor de um certo quanto, e dependendo dele, retornando de 1 a 3 

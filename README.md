## Operações com dataframes usando pandas

    1. Visualização do conteúdo do dataframe


```python
import pandas as pd

df = pd.read_csv('https://raw.githubusercontent.com/justmarkham/DAT8/master/data/chipotle.tsv', sep='\t')

df.info()   #mostra o tipo de dados de cada coluna
df.describe()   #mostra estatísticas descritivas de cada coluna
df.head()   #mostra as 5 primeiras linhas
df.tail()   #mostra as 5 últimas linhas
```

    2. Ver e filtrar colunas da tabela


```python
#carregar uma tabela do próprio pandas para testes
df = pd.read_csv('https://raw.githubusercontent.com/justmarkham/DAT8/master/data/chipotle.tsv', sep='\t')
#com o df.columns podemos ver o nome de todas as colunas do dataframe
df.columns
#na linha abaixo podemos filtrar quais colunas aparecerão no dataframe. Caso queira ocultar alguma, basta apagar da lista
df = df[['order_id', 'quantity', 'item_name', 'choice_description',
       'item_price']]
df
```

    2.1. Filtrar valores nas colunas


```python
df = pd.read_csv('https://raw.githubusercontent.com/justmarkham/DAT8/master/data/chipotle.tsv', sep='\t')

#verificar valores únicos da coluna quantity
df['quantity'].unique()

df[df['quantity'] == 2]

#filtrar a coluna quantity para mostrar apenas os valores 2 e a coluna item_name para mostrar apenas os valores Chicken Bowl
df[(df['quantity'] == 2) & (df['item_name'] == 'Chicken Bowl')]

#usar o iloc para mostrar as 50 primeiras linhas da tabela
df.iloc[0:50]

#usar o loc para mostrar Item_name == steak burrito
df.loc[df['item_name'] == 'Steak Burrito']

#usar o loc para mostrar Item_name == steak burrito e quantity == 2
df.loc[(df['item_name'] == 'Steak Burrito') & (df['quantity'] == 2)]

#usar o loc para mostrar Item_name == steak burrito e quantity == 2 e order_id == 1
df.loc[(df['item_name'] == 'Steak Burrito') & (df['quantity'] == 2) & (df['order_id'] == 264)]

#filtrar valores entre 2 e 5 na coluna quantity usando o loc
df.loc[(df['quantity'] >= 2) & (df['quantity'] <= 5)]

#organizar o dataframe pela coluna quantity
df.sort_values('quantity')
```

    3. Ver e apagar valores nulos


```python
df = pd.read_csv('https://raw.githubusercontent.com/justmarkham/DAT8/master/data/chipotle.tsv', sep='\t')
df.isnull().sum()
#apagar os valores nulos de uma coluna específica, neste caso, a coluna choice_description
df.dropna(subset=['choice_description']).reset_index(drop=True)
#apagar todos os valores nulos de todo o dataframe
df.dropna(how='all').reset_index(drop=True)

#apagar a coluna choice_description
df.drop('choice_description', axis=1) #o axis=1 é para dizer que é uma coluna, desta forma, apagando a coluna
                                        #se for axis=0, apaga a linha
```

    4. Mudando o tipo de dado de uma coluna


```python
df = pd.read_csv('https://raw.githubusercontent.com/justmarkham/DAT8/master/data/chipotle.tsv', sep='\t')

#transformar a coluna quantity em float
df['quantity'] = df['quantity'].astype(float)
#transformar a coluna quantity em string
#df['quantity'] = df['quantity'].astype(str)
#transformar a coluna quantity em inteiro novamente
df['quantity'] = df['quantity'].astype(int)


# OBS extra:
#para colocar o tipo de dado correto na coluna item_price, é preciso manipular a string e retirar o símbolo $ e depois transformar em float
df['item_price'] = df['item_price'].str.replace('$', '').astype(float)
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4622 entries, 0 to 4621
    Data columns (total 5 columns):
     #   Column              Non-Null Count  Dtype  
    ---  ------              --------------  -----  
     0   order_id            4622 non-null   int64  
     1   quantity            4622 non-null   int64  
     2   item_name           4622 non-null   object 
     3   choice_description  3376 non-null   object 
     4   item_price          4622 non-null   float64
    dtypes: float64(1), int64(2), object(2)
    memory usage: 180.7+ KB


    /tmp/ipykernel_16277/2572900213.py:13: FutureWarning: The default value of regex will change from True to False in a future version. In addition, single character regular expressions will *not* be treated as literal strings when regex=True.
      df['item_price'] = df['item_price'].str.replace('$', '').astype(float)


    5. Manipulação de string no dataframe


```python
df = pd.read_csv('https://raw.githubusercontent.com/justmarkham/DAT8/master/data/chipotle.tsv', sep='\t')

df['item_name'] = df['item_name'].str.replace(' ', '_')

#mostrar somente as 5 primeiras letras de cada item_name
df['item_name'] = df['item_name'].str.slice(0,5)
```

    5.1. Mudando o nome das colunas


```python
df = pd.read_csv('https://raw.githubusercontent.com/justmarkham/DAT8/master/data/chipotle.tsv', sep='\t')

#mudar o nome da coluna item_price para preco_do_item
df.rename(columns={'item_name': 'nome_do_item'}, inplace=True)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>order_id</th>
      <th>quantity</th>
      <th>nome_do_item</th>
      <th>choice_description</th>
      <th>item_price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
      <td>Chips and Fresh Tomato Salsa</td>
      <td>NaN</td>
      <td>$2.39</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>Izze</td>
      <td>[Clementine]</td>
      <td>$3.39</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>1</td>
      <td>Nantucket Nectar</td>
      <td>[Apple]</td>
      <td>$3.39</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>1</td>
      <td>Chips and Tomatillo-Green Chili Salsa</td>
      <td>NaN</td>
      <td>$2.39</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2</td>
      <td>Chicken Bowl</td>
      <td>[Tomatillo-Red Chili Salsa (Hot), [Black Beans...</td>
      <td>$16.98</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4617</th>
      <td>1833</td>
      <td>1</td>
      <td>Steak Burrito</td>
      <td>[Fresh Tomato Salsa, [Rice, Black Beans, Sour ...</td>
      <td>$11.75</td>
    </tr>
    <tr>
      <th>4618</th>
      <td>1833</td>
      <td>1</td>
      <td>Steak Burrito</td>
      <td>[Fresh Tomato Salsa, [Rice, Sour Cream, Cheese...</td>
      <td>$11.75</td>
    </tr>
    <tr>
      <th>4619</th>
      <td>1834</td>
      <td>1</td>
      <td>Chicken Salad Bowl</td>
      <td>[Fresh Tomato Salsa, [Fajita Vegetables, Pinto...</td>
      <td>$11.25</td>
    </tr>
    <tr>
      <th>4620</th>
      <td>1834</td>
      <td>1</td>
      <td>Chicken Salad Bowl</td>
      <td>[Fresh Tomato Salsa, [Fajita Vegetables, Lettu...</td>
      <td>$8.75</td>
    </tr>
    <tr>
      <th>4621</th>
      <td>1834</td>
      <td>1</td>
      <td>Chicken Salad Bowl</td>
      <td>[Fresh Tomato Salsa, [Fajita Vegetables, Pinto...</td>
      <td>$8.75</td>
    </tr>
  </tbody>
</table>
<p>4622 rows × 5 columns</p>
</div>



    6. Criando novas colunas


```python
df = pd.read_csv('https://raw.githubusercontent.com/justmarkham/DAT8/master/data/chipotle.tsv', sep='\t')

df['multi_quantity'] = df['quantity'] * 2
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>order_id</th>
      <th>quantity</th>
      <th>item_name</th>
      <th>choice_description</th>
      <th>item_price</th>
      <th>multi_quantity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
      <td>Chips and Fresh Tomato Salsa</td>
      <td>NaN</td>
      <td>$2.39</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>Izze</td>
      <td>[Clementine]</td>
      <td>$3.39</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>1</td>
      <td>Nantucket Nectar</td>
      <td>[Apple]</td>
      <td>$3.39</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>1</td>
      <td>Chips and Tomatillo-Green Chili Salsa</td>
      <td>NaN</td>
      <td>$2.39</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2</td>
      <td>Chicken Bowl</td>
      <td>[Tomatillo-Red Chili Salsa (Hot), [Black Beans...</td>
      <td>$16.98</td>
      <td>4</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4617</th>
      <td>1833</td>
      <td>1</td>
      <td>Steak Burrito</td>
      <td>[Fresh Tomato Salsa, [Rice, Black Beans, Sour ...</td>
      <td>$11.75</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4618</th>
      <td>1833</td>
      <td>1</td>
      <td>Steak Burrito</td>
      <td>[Fresh Tomato Salsa, [Rice, Sour Cream, Cheese...</td>
      <td>$11.75</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4619</th>
      <td>1834</td>
      <td>1</td>
      <td>Chicken Salad Bowl</td>
      <td>[Fresh Tomato Salsa, [Fajita Vegetables, Pinto...</td>
      <td>$11.25</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4620</th>
      <td>1834</td>
      <td>1</td>
      <td>Chicken Salad Bowl</td>
      <td>[Fresh Tomato Salsa, [Fajita Vegetables, Lettu...</td>
      <td>$8.75</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4621</th>
      <td>1834</td>
      <td>1</td>
      <td>Chicken Salad Bowl</td>
      <td>[Fresh Tomato Salsa, [Fajita Vegetables, Pinto...</td>
      <td>$8.75</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>4622 rows × 6 columns</p>
</div>



7. Abrindo vários arquivos .csv de um diretório como um único dataframe


```python
# vamos utilizar a biblioteca glob para listar os arquivos .csv
import glob

arquivos = glob.glob('diretorio/*.csv')

#abrir todos os arquivos .csv em um só dataframe
df = pd.concat([pd.read_csv(arquivo) for arquivo in arquivos])
```

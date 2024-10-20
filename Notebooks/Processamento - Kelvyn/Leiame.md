
# Tratamento de dados Json

# URLs dos arquivos JSON no GitHub

url_viniferas = 'https://raw.githubusercontent.com/Data-Analitycs-Pos-Tech-Fiap/DadosVinifera/refs/heads/main/bases_de_dados/dados_processamento/viniferas.json'
url_americanas = 'https://raw.githubusercontent.com/Data-Analitycs-Pos-Tech-Fiap/DadosVinifera/refs/heads/main/bases_de_dados/dados_processamento/americanas.json'
url_mesa = 'https://raw.githubusercontent.com/Data-Analitycs-Pos-Tech-Fiap/DadosVinifera/refs/heads/main/bases_de_dados/dados_processamento/mesa.json'

# Dicionário para armazenar os DataFrames
dataframes = {}

# Lista de URLs para iteração
urls = {
    'viniferas': url_viniferas,
    'americanas': url_americanas,
    'mesa': url_mesa,
}

# Fazer a requisição HTTP para cada URL
for key, url in urls.items():
    response = requests.get(url)

    # Verificar se a requisição foi bem-sucedida
    if response.status_code == 200:
        # Tentativa de decodificação do JSON
        try:
            # Capturando o texto da resposta
            content = response.text

            # Transformando o conteúdo em uma lista de objetos JSON
            json_data = [json.loads(line) for line in content.strip().splitlines()]

            # Transformar o JSON em um DataFrame (estrutura tabular)
            dataframes[key] = pd.json_normalize(json_data)

            # Exibir as primeiras linhas do DataFrame para visualização
            print(f"DataFrame para {key}:")
            print(dataframes[key].head())
        except ValueError as e:
            print(f"Erro ao decodificar JSON para {key}: {e}")
            print("Conteúdo da resposta:")
            print(response.text)  # Imprime o conteúdo bruto da resposta
    else:
        print(f"Erro ao acessar {key}. Código de status: {response.status_code}")

#  PLN - Coleta e Pré-processamento de Dados: Letterboxd

Este repositório contém a implementação da primeira etapa prática da disciplina de Processamento de Linguagem Natural (PLN), com o objetivo de construir e tratar uma base de dados textuais.

**Equipe:** Aline Sabel e Henrique André Oneda
**Fonte de Dados:** [Letterboxd](https://letterboxd.com/) através da biblioteca [`letterboxdpy`](https://github.com/nmcassa/letterboxdpy)

## Funcionalidades e Pipeline

O script em Python executa as seguintes etapas para a criação e preparação da base de dados:

* **Coleta Ampliada:** Busca de filmes populares e listas por múltiplos gêneros (ação, comédia, drama, terror, romance, etc.) para garantir maior volume e variedade de resenhas.
* **Limpeza de Ruído:** Remoção de tags HTML, URLs e espaçamentos excessivos dos textos brutos.
* **Detecção de Idioma:** Identificação da linguagem da resenha (sendo uma plataforma multilíngue) para a aplicação correta das técnicas subsequentes.
* **Tokenização e Normalização:** Conversão dos textos para caixa baixa, extração de tokens utilizando a biblioteca NLTK e remoção de números e pontuações.
* **Remoção de Stopwords:** Filtragem de palavras vazias utilizando os dicionários apropriados para cada idioma detectado.
* **Stemming (Bônus):** Redução das palavras aos seus radicais, aplicando o `RSLPStemmer` para português e o `SnowballStemmer` para inglês.

## Dicionário de Dados

Ao final da execução, o projeto exporta os dados processados em um arquivo CSV estruturado da seguinte forma:

| Coluna | Tipo | Descrição |
|---|---|---|
| `filme` | str | Título do filme |
| `filme_slug` | str | Slug do filme no Letterboxd |
| `ano_lancamento` | int | Ano de lançamento do filme |
| `generos` | list[str] | Gêneros do filme |
| `diretores` | list[str] | Diretor(es) do filme |
| `nota_media_letterboxd` | float | Nota média do filme na plataforma |
| `usuario` | str | Nome de usuário de quem escreveu a resenha |
| `nota_resenha` | float | Nota dada pelo usuário naquela resenha (0.5 a 5.0) |
| `link` | str | URL da resenha |
| `resenha_original` | str | Texto bruto da resenha, como coletado (primeiro parágrafo) |
| `resenha_limpa` | str | Texto após remoção de ruído (HTML, URLs, espaços) |
| `idioma` | str | Idioma detectado (`pt`, `en`, outro código ISO ou `desconhecido`) |
| `tokens` | list[str] | Tokens após normalização, tokenização e remoção de stopwords |
| `tokens_stemizados` | list[str] | Tokens após stemming (bônus) |

## Limitações Conhecidas

* O método oficial da biblioteca para buscar resenhas por filme (`Movie(slug).get_reviews()`) está inoperante, exigindo a extração da seção "Popular Reviews", que não oferece suporte à paginação.
* Devido a restrições do parser da biblioteca atual, apenas o primeiro parágrafo de cada avaliação é coletado, truncando textos mais extensos.
* Filmes com páginas indisponíveis ou que apresentam erros de acesso são automaticamente ignorados e registrados para não interromper a coleta.

## Instalação e Execução

Para rodar este notebook em seu ambiente, instale as dependências necessárias executando os comandos abaixo:

```bash
# Instala a versão mais recente do scraper direto do repositório
pip install -q git+https://github.com/nmcassa/letterboxdpy.git

# Instala as bibliotecas de PLN e manipulação de dados
pip install -q nltk langdetect unidecode pandas tqdm
```

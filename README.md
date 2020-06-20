Curso de Especialização de Inteligência Artificial Aplicada

Setor de Educação Profissional e Tecnológica - SEPT

Universidade Federal do Paraná - UFPR

---

**IAA003 - Linguagem de Programação Aplicada**

Prof. Alexander Robert Kutzke

# Exercício de implementação do algoritmo Naive Bayes

Altere o código do arquivo [spam_classifier.py](spam_classifier.py) para adicionar
algumas das seguintes funcionalidades:

- Utilizar a biblioteca NumPy se considerar pertinente;
- Utilizar a biblioteca Pandas se considerar pertinente;
- Analisar o conteúdo da mensagem e não apenas o Assunto;
- Considerar apenas palavras que aparecem um número mínimo de vezes 
  (`min_count`);
- Utilizar apenas radicais das palavras (pesquise por "Porter Stemmer");
- Considerar não apenas presença de palavras, mas outras características:
  - Por exemplo, se a mensagem possuí números:
    - A função `tokenizer` pode retornar *tokens* especiais para isso (por exemplo: 
      `contains:number`).

**Comente seu código indicando as alterações realizadas.**

---

# Alterações realizadas

Foi adicionado a biblioteca `nltk` para remover as stopwords

- Antiga função `tokenize`:
    ```
    def tokenize(message): 
    message = message.lower()                      
    all_words = re.findall("[a-z0-9']+", message)   
    return set(all_words)
    ```
- Atual função `tokenize`:
    ```
    import nltk
    from nltk.corpus import stopwords
    from nltk.tokenize import RegexpTokenizer, word_tokenize
    
    # para baixar a biblioteca de stop words utilizar: nltk.download()
    
    def tokenize(message):
        message = message.lower()  # converte para lower case
        tokenizer = RegexpTokenizer(r"\w+") # pega somente as palavras
        text_tokens = tokenizer.tokenize(message) # transforma em tokens
        stopWords = stopwords.words("english") # pega apenas as stop words em inglês para ficar mais rapido
        setStopWords = set(stopWords) # remove stop words repetidas
        tokens_without_sw = [word + " " for word in text_tokens if not word in setStopWords and word != "com"] # remove as stop words
        text = "".join(tokens_without_sw) # transforma em string
        all_words = re.findall("[a-z0-9']+", text)  # extrai as palavras
        return set(all_words)  # remove duplicados
    ```


A proporção de base para testes e treino foi reduzida para `0.7`, e a probabilidade de spam aumentada para `0.7`

Com isso tivemos a seguinte melhoria:

> Resultado do algoritmo original (Utilizando o Subject dos E-mails):

|           | True | False |
|-----------|------|-------|
| **True**  | 101  | 38    |
| **False** | 33   | 704   |

> Resultado do algoritmo atual (Utilizando o Subject dos E-mails):

|           | True | False |
|-----------|------|-------|
| **True**  | 105  | 71    |
| **False** | 14   | 871   |

> Resultado com o algoritmo atual (Utilizando o From dos E-mails):

|           | True | False |
|-----------|------|-------|
| **True**  | 107  | 66    |
| **False** | 12   | 883   |

> Portanto tivemos uma pequena melhora no algoritmo com a utilização do tokenizer e remoção das stopwords, e a melhora fica mais visível a partir do momento que utilizamos o `From` dos e-mails ao invés do `Subject`

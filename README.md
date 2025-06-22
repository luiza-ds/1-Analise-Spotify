# 🎶 MÚSICAS MAIS OUVIDAS NO SPOTIFY

Nessa análise através das músicas mais ouvidas do Spotify, eu busco entender como se comportam as músicas através dos anos e como fatores como Dançabilidade e Batidas Por Minuto resultam em uma música de sucesso ou não. Dentro dessa análise você pode encontrar insights das músicas mais ouvidas, gráficos de dispersão, correlação e curiosidades musicais de (talvez) seus artistas favoritos. 

Software: R

Banco de Dados:
[Spotify Most Streamed Songs.csv](https://github.com/user-attachments/files/17261675/Spotify.Most.Streamed.Songs.csv)


## 
Nessa análise do conjunto de dados das músicas mais ouvidas no Spotify trago insights valiosos sobre a plataforma e suas músicas de sucesso.


Comecei analisando o conjunto como um todo, verificando as variáveis existente e as suas características.
Retirei algumas colunas que não nos importavam para essa análise, tratei as variávies numéricas que não estavam de acordo com essa classe e as de fator que também não estavam coerentes. 

Plotei um gráfico para saber a distribuição dos streams como um todo dentro do nosso banco de dados. 

![Gráfico de Distribuição de Streams](https://github.com/user-attachments/assets/652f0631-2455-4c9b-b967-96541bcc084e)

Uma distribuição nada normal e com muitos pulos. 

Em seguida, ordenei todas as músicas de maneira decrescente em relação aos streams. Ou seja, da mais ouvida até a menos. 
Após essa ordenação, selecionei o top 10 e analisei se havia correlação entre a música estar em muitas playlists do Spotify e possuir muitos streams. 
E plotei um gráfico da quantidade de Streams e da quantidade de playlists inseridas para ser analisada.

![Streams X Playlists](https://github.com/user-attachments/assets/c4b2feec-ddf7-47d1-9592-56547a69702b)

Calculei também a correlação entre essas duas variáveis:
Com uma resposta de 0.5139794, podemos concluir que a correlação é moderada.
Podemos analisar que "Blinding Lights" do The Weeknd se mantém como a musica mais ouvida e em mais playlists no Spotify também.

Outro caso: "One Dance" do Drake é a segunda musica mais posta em playlists no spotify, porém é a sexta música mais ouvida no Spotify.

Outro: "Starboy" segue em 4° lugar com mais playlists porém é a décima música mais ouvida.

Assim, é possível analisar que há uma variação de popularidade. 
Com exceção de "Blinding Lights", não podemos afirmar com certeza que as
músicas com mais playlists no spotify serão aquelas com mais streams.


Agora, analisaremos se houve um ano com mais hits musicais. 

Nessa parte, transformei os dados da variável "Streams" para números com vírgulas e pontos. (No banco de dados original, estava em números corridos.)
E fiz uma inferência no banco de dados com as 250 primeiras músicas para analisarmos a incidência por ano. 

Plotei com um gráfico de barras: 

![Top250 X Por ano](https://github.com/user-attachments/assets/421f5af7-7ce0-4e59-91cf-c89e5e3f23a8)

A partir desse gráfico, vi que os anos que atecedem 2010 possuem poucas músicas lançadas.
Podendo indicar uma mudança na indústria musical ou na popularidade das plataformas de streaming.

Iremos verificar agora o ano com mais hits.
(Tirei as vírgulas e pontos dos dados Streams, para essa análise)

Usando o pacote "Dply" no R, entramos no nosso banco de dados, agrupamos no novo data frame "streams_por_ano" por ano e logo após sumerizamos cada uma, nos devolvendo a soma de streams por ano.
Assim, descobriremos qual ano possui mais streams e por consequencia o ano com mais hits. 

![Gráfico stream X ano](https://github.com/user-attachments/assets/0c0b4574-9aa3-49e5-a3cb-fb07ff777029)

Observando pelo gráfico acima, podemos perceber uma maior concetração de streams após os anos 2000.
Porém, uma maior quantidade a partir de 2010, então nosso ano com mais hits estará nesse grupo.
Vamos abrir um novo data frame somente com os anos após 2010 com o total de streams. 

![Ano com mais streams](https://github.com/user-attachments/assets/3f63fe76-e685-4ea4-80e7-1705ac85927d)

Assim, concluimos que em 2022 foi o ano com mais Streams na história do Spotify até o momento com 1.164.023.779,62 de streams. 
Podendo ser considerado o ano com mais hits acumulados.

Em seguida, plotei um gráfico com a média de streams por ano:

![Media de Stream  x  Ano](https://github.com/user-attachments/assets/d397cdc6-a0ad-47ac-84aa-7755dd985e6b)

Podemos analisar que a media de streams por ano foi decaindo após o ano de 2017. Podemos pensar que houveram mais musicas que alcançaram um sucesso global entre 2016 e 2019, no entando essas mais recentes possam não ter tido tempo o suficiente para acumularem streams, entre outros diversos motivos.


E por último iremos fazer correlações das músicas mais ouvidas com os fatores de "Danceability" e "BPM" para verificarmos se há relação entre a música ser mais animada ou não para ter sucesso na plataforma digital. 

![Gráfico de correlação entre StreamXBPM](https://github.com/user-attachments/assets/3e0eb61c-a8d4-4bdc-a3b9-60970773529c)

![Correlação entre Streams X Danceeability](https://github.com/user-attachments/assets/c4b54985-3f2d-4eba-abdb-1bef43ca581a)

Com Correlações muito baixas, não é possível afirmamos que a música terá sucesso no Spotify pelo os seus BPM ou Danceability altos.
Há uma dispersão bem considerável nessas variáveis, não sendo possível provar com 100% que só porquê ela possui um alto BPM, será um grande hit na plataforma. 



#BÔNUS

Fiz uma análise somente puxando os dados da minha cantora favorita: SZA.

Analisando as suas músicas mais ouvidas! 

![Músicas mais ouvidas da SZA](https://github.com/user-attachments/assets/0a93a211-d93d-4f9f-a45b-7ada8ca8c57e)

E o grande sucesso dela "Kill Bill" com cerca de 1.1B (Hoje, 2024, com 2B) de streams. Esperado porém não surpresa. 



OBRIGADA! 

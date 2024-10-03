# 1-Projeto
Most Streamed Songs on Spotify

library(ggplot2)
library(dplyr)
library(scales) #biblioteca para a escala de numeros grandes

musicas <- read.csv("Spotify Most Streamed Songs.csv")

str(musicas)
summary(musicas)
musicas <- na.omit(musicas)

musicas$mode <- as.factor(musicas$mode)
musicas$key <- as.factor(musicas$key)

head(musicas)
#tirei colunas do dia e do ano lançados, por enquanto nao sera necessário
musicas <- subset(musicas, select = -c(released_month, released_day, acousticness_.,instrumentalness_.,liveness_.,speechiness_.))

#transformei a variavel streams em numerico
musicas$streams <- as.numeric(musicas$streams)


#plotando um grafico para analisar a distribuição de streams 
#com um espaço de 50 milhoes de streams

ggplot(musicas, aes(x = streams)) +
  geom_histogram(binwidth = 50000000, fill = "blue", color = "black") +
  theme_minimal() +
  labs(title = "Distribuição de Streams das Músicas no Spotify",
       x = "Streams",
       y = "Frequência")



#ordenei todas as musicas de forma descrescente
musicas <- musicas[order(-musicas$streams), ]

#peguei as 10 primeiras musicas mais ouvidas
top_10_musicas <- head(musicas, 10)

#fazendo um grafico das top10 musicas, em ordem das que estão em mais playlists mo spotify
ggplot(top_10_musicas, aes(x = reorder(track_name, in_spotify_playlists), y = in_spotify_playlists)) +
  geom_bar(stat = "identity", fill = "darkorange") +
  coord_flip() +
  labs(title = "Top 10 Músicas mais Streamadas no Spotify",
       x = "Nome da Música",
       y = "Quantidade em Playlists do Spotify") +
  theme_light()


#calcularemos a correlação entre a quantidade de playlist com a quantidade de streams das musicas
cor(top_10_musicas$in_spotify_playlists, top_10_musicas$streams)
#com uma resposta de 0.5139794, podemos concluir que a correlação é moderada

#Podemos analisar que mesmo "Blinding Lights" se mantem como a musica mais ouvida e em mais playlists no spotify.
#Outro caso: "One Dance" do Drake é a segunda musica mais posta em playlists no spotify, porém é a sexta música mais streamada.
#Outro: "Starboy" segue em 4 lugar com mais playlists porém é a décima música mais ouvida.
#Podemos analisar que há uma variação de popularidade. Com exceção de "Blinding Lights", não podemos afirmar com certeza que as
#músicas com mais playlists no spotify serão aquelas com mais streams.



#Teve um ano com mais hits?

musicas$streams <- format(musicas$streams, big.mark = ",", scientific = FALSE)

#farei uma inferencia com uma amostra aleatoria maior 

top250 <- head(musicas, 250)

str(musicas)

ggplot(top250, aes(x= released_year))+
  geom_bar(fill = "skyblue", color = "black")+
  labs(title = "Número de Músicas lançadas por ano (Top 250)",
       x = "Ano de Lançamento",
       y= "Frequência")+
  theme_bw()

#fazendo um grafico teste com 250 faixas musicais, como inferencia, vi que os anos que atecedem 2010 possuem 
#poucas músicas lançadas.
#Pode indicar uma mudança na indústria musical ou na popularidade das plataformas de streaming.


#Iremos verificar agora o ano com mais Hits. 

musicas$streams <- as.numeric(musicas$streams)

#usando o pipe, entramos no nosso banco de dados, agrupamos por ano e logo apos sumerizamos cada stream por ano. Nós devolvendo a soma de streams por ano.
#Assim, descobriremos qual ano possui mais streams e por consequencia o ano com mais hits. 

ano_mais_hits <- musicas %>%
  group_by(released_year) %>%
  summarise(streams_por_ano = sum(streams, na.rm = TRUE))
#criei um novo data frame somente com os anos e o total de streams

#grafico de linhas para avaliar o comportamento de streams por ano
ggplot(ano_mais_hits, aes(x = released_year, y = streams_por_ano))+
  geom_point(col = "darkblue")+
  geom_line()+
  labs(title = "Quantidade de Streams por Ano",
       x = "Ano de Lançamento",
       y = "Total de Streams")+
  theme_bw()+
  theme(axis.text.x = element_text(angle = 45, hjust = 1))

#Observando pelo gráfico acima, podemos perceber uma maior concetração de streams após 2000-2010

# Vamos calcular o total de streams para as músicas lançadas após 2010.
ano_mais_hits_2010 <- musicas_2010 %>%
  group_by(released_year) %>%
  summarise(total_streams_2010 = sum(streams, na.rm = TRUE))

ggplot(ano_mais_hits_2010, aes(x = released_year, y = total_streams_2010)) +
  geom_bar(stat = "identity", fill = "purple", col = "black") +
  labs(title = "Total de Streams de Músicas Lançadas a partir de 2010",
       x = "Ano de Lançamento",
       y = "Total de Streams") +
  theme_bw()

#Assim, concluimos que em 2022 foi o ano com mais Streams na história do Spotify até o momento com 1.164.023.779,62 de streams. 



#####Média de Streams por ano


#Faremos um filtro para criar um data frame somente com as musicas de 2010 e posteriormente
musicas_2010 <- musicas %>%
  filter(released_year >= 2010)

#Estamos agrupando por ano e calculando a media de streams de cada ano e criando a variável "media_streams" no nosso novo data frame. 
media_streams_por_ano <- musicas_2010 %>%
  group_by(released_year) %>%
  summarise(media_streams = mean(streams, na.rm = TRUE))

#Plotando a media de streams por ano, temos: 

ggplot(media_streams_por_ano, aes(x = released_year, y = media_streams)) +
  geom_line(color = "red") +
  geom_point(color = "darkblue") +
  labs(title = "Média de Streams por Ano",
       x = "Ano de Lançamento",
       y = "Média de Streams") +
  scale_y_continuous(labels = comma) +
  theme_bw()

#Podemos analisar que a media de streams por ano foi decaindo após o ano 
#de 2017. Podemos pensar que houveram mais musicas que alcançaram um sucesso global
#ente 2016 e 2019, no entando essas mais recentes possam não ter tido tempo o suficiente para acumularem streams, entre outros diversos motivos.



##Músicas Mais ouvida de cada Ano

#*Utilizando a métrica de 2010 pois é melhor para que o nosso gráfico fique visível.

musicas_mais_ouvidas_ano <- musicas_2010 %>%
  group_by(released_year) %>%
  slice_max(streams, n = 1) %>%
  select(released_year, track_name, artist.s._name, streams)

ggplot(musicas_mais_ouvidas_ano, aes(x=released_year, y=streams))+
  geom_point(fill = "blue",size = 1)+
  geom_text(aes(label = track_name, size = 1))+
  scale_y_continuous(labels = comma) +
  labs(title = "Músicas mais Ouvidas de cada Ano",
       x = "Ano de Lançamento",
       y= "Streams")+
  theme_bw()


#E por último iremos fazer correlações das músicas mais ouvidas com "Danceability" e "BPM".
musicas$bpm <- as.numeric(musicas$bpm)
musicas$danceability_. <- as.numeric(musicas$danceability_.)

sum(is.na(musicas$streams))
musicas <- na.omit(musicas)


cor(musicas$streams, musicas$danceability)
cor(musicas$streams, musicas$bpm)


ggplot(musicas, aes(x = danceability_., y = streams)) +
  geom_point(color = "blue", alpha = 0.5) +
  scale_y_continuous(labels = comma) +
  labs(title = "Relação entre Streams e Danceability",
       x = "Danceability",
       y = "Streams")+
  theme_bw()

ggplot(musicas, aes(x = bpm, y = streams)) +
  geom_point(color = "blue", alpha = 0.5) +
  scale_y_continuous(labels = comma) +
  labs(title = "Relação entre Streams e BPM",
       x = "BPM",
       y = "Streams")+
  theme_bw()


#---------------------------------------------------------
#Análise SZA

sza <- musicas %>%
  filter(musicas$artist.s._name == "SZA")

sza <- sza[order(-sza$streams), ]

ggplot(sza, aes(x = reorder(track_name, streams), y = streams)) +
  geom_bar(stat = "identity", fill = "purple", col = "black") +
  scale_y_continuous(labels = comma) +
  coord_flip() +  # Inverte os eixos para melhor visualização
  labs(title = "Streams das Músicas da SZA",
       x = "Música",
       y = "Número de Streams") +
  theme_bw()
  

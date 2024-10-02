# 1-Projeto
Most Streamed Songs on Spotify

Nessa análise através das músicas mais ouvidas do Spotify, eu busco entender como se comportam as músicas através dos anos e como fatores como Dançabilidade e Batidas Por Minuto resultam em uma música de sucesso ou não. Dentro dessa análise você pode encontras insights das músicas mais ouvidas, gráficos de dispersão, correlação e curiosidades musicais de (talvez) seus artistas favoritos. 

musicas <- read.csv("Spotify Most Streamed Songs.csv")

str(musicas)

attach(musicas)

summary(musicas)

na.omit(musicas)

musicas <- musicas[,-25]
musicas$mode <- as.factor(musicas$mode)
musicas$key <- as.factor(musicas$key)

head(musicas)
#tirei colunas do dia e do ano lançados, por enquanto nao sera necessário
musicas <- subset(musicas, select = -c(released_month, released_day))

#transformei a variavel streams em numerico
musicas$streams <- as.numeric(musicas$streams)

#ordenei todas as musicas de forma descrescente
musicas_ordenadas <- musicas[order(-musicas$streams), ]

#peguei as 10 primeiras musicas mais ouvidas
top_10_musicas <- head(musicas_ordenadas, 10)

#farei uma inferencia com uma amostra aleatoria maior 

top250 <- head(musicas_ordenadas, 250)


#fazendo um grafico das top10 musicas, em ordem das que estão em mais playlists mo spotify
ggplot(top_10_musicas, aes(x = reorder(track_name, in_spotify_playlists), y = in_spotify_playlists)) +
  geom_bar(stat = "identity", fill = "darkorange") +
  coord_flip() +
  labs(title = "Top 10 Músicas mais Streamadas no Spotify",
       x = "Nome da Música",
       y = "Quantidade em Playlists do Spotify") +
  theme_light()

#Podemos analisar que mesmo "Blinding Lights" se mantem como a musica mais ouvida e em mais playlists no spotify.
#Outro caso: "One Dance" do Drake é a segunda musica mais posta em playlists no spotify, porém é a sexta música mais streamada.
#Outro: "Starboy" segue em 4 lugar com mais playlists porém é a décima música mais ouvida.
#Podemos analisar que há uma variação de popularidade. Com exceção de "Blinding Lights", não podemos afirmar com certeza que as
#músicas com mais playlists no spotify serão aquelas com mais streams.



#Teve um ano com mais hits?
musicas_2022ordenadas <- musicas_2022[order(-musicas_2022$streams), ]

musicas$streams <- format(musicas$streams, big.mark = ",", scientific = FALSE)
str(musicas)

musicas_2022ordenadas$streams <- format(musicas_2022ordenadas$streams, big.mark = ",", scientific = FALSE)

ggplot(top250, aes(x= released_year))+
  geom_bar()

#fazendo um grafico teste com 250 faixas musicais, como inferencia, vi que os anos que atecedem 2010 possuem 
#poucas músicas lançadas. 


#fazendo um filtro das musicas lançadas em 2010 ou após esse ano
musicas_2010 <- musicas %>%
  filter(released_year >= 2010)

ggplot(musicas_2010, aes(x= released_year))+
  geom_bar(fill = "green")+
  labs(title = "Músicas lançadas a partir de 2010",
       x = "Ano de Lançamento",
       y = "Quantidade de Músicas")+
  theme_bw()

table(musicas_2010$released_year)
#Temos 402 músicas lançadas em 2022 que estão no nosso banco de dados das músicas mais streamadas do Spotify


#Agora, faremos algumas analises estatísticas a partir dessas informações. 

402/953
#42% é equivalente a porcentagem de músicas do ano de 2022 que estão entre as mais streamadas do spotify. 
#Equivalente a quase a metade do meu banco de dados. 


#####Média de Streams por ano

musicas_2010$streams <- as.numeric(musicas_2010$streams)

media_streams_por_ano <- musicas_2010 %>%
  group_by(released_year) %>%
  summarise(media_streams = mean(streams, na.rm = TRUE))

ggplot(media_streams_por_ano, aes(x = released_year, y = media_streams)) +
  geom_line(color = "blue") +
  geom_point() +
  labs(title = "Média de Streams por Ano",
       x = "Ano de Lançamento",
       y = "Média de Streams") +
  theme_minimal()



##Músicas Mais ouvida de cada Ano

musicas_mais_ouvidas_ano <- musicas %>%
  group_by(released_year) %>%
  slice_max(order_by = streams, n = 1, with_ties = FALSE) 

ggplot(musicas_mais_ouvidas_ano, aes(x = released_year, y = streams, label = track_name)) +
  geom_line() +
  geom_point() +
  geom_text(aes(label = track_name), vjust = -0.5, size = 3) +  # Adiciona o nome da música no gráfico
  labs(title = "Música mais ouvida por ano",
       x = "Ano de Lançamento",
       y = "Número de Streams") +
  theme_bw()

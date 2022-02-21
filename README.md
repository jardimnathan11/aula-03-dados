# aula-03-dados verão
library (rvest)
library(tidyverse)
library(dplyr)
library(janitor)
library(lubridate)

link= 'https://www.imdb.com/chart/top/'
 top=read_html(link)
  top_IMDB= top %>%
  html_nodes("strong , .secondaryInfo , #main a") %>% 
     ##select table element
    html_text()
  top_IMDB<-top_IMDB[-1]#limpar linha quebrada
  top_IMDB=matrix(top_IMDB,ncol=4, byrow = T)#transformar texto em tabela
   top_IMDB=as.data.frame(top_IMDB)
  top_IMDB<-top_IMDB[-251,]#tirar linha quebrada(veio sonho de liberdade repetido por motivo desconhecido)
colnames(top_IMDB)<-c('rank','title','year', 'rating')   
   top_IMDB$rank<-1:250#colocar o rank numerico
   
   ggplot(top_IMDB, 
          aes(x=rank, y =rating)) +  
     geom_point(color= 'red')+
     labs(title = "IMDB rating by rank", y = 'rating', x='rank')
   ggsave("IMDB top 250 rating by rank.png")
   top50_IMDB=top_IMDB[1:50, 1:4]# fazer top 50 para a imagem mais clean
   ggplot(top50_IMDB, 
          aes(x=rating, y =year)) +  
     geom_point(color= 'blue')+
     labs(title = "IMDB top 50 year by rating ", y = 'year', x='raating')
   ggsave("IMDB top 50 year by rating.png")

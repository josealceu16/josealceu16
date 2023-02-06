Olá, eu sou o José Alceu 👋
📚 Atualmente estou estudando Java 
![Anurag's GitHub stats](https://github-readme-stats.vercel.app/api?username=josealceu16&theme=transparent&show_icons=true)
import  {  renderTopLanguages  ​​}  de  "../src/cards/top-languages-card.js" ;
importar  {  lista negra  }  de  "../src/common/blacklist.js" ;
importar  {
  valor da braçadeira ,
  CONSTANTES ,
  parseArray ,
  parseBoolean ,
  renderErro ,
}  from  "../src/common/utils.js" ;
importar  {  fetchTopLanguages  ​​}  de  "../src/fetchers/top-languages-fetcher.js" ;
importar  {  isLocaleAvailable  }  de  "../src/translations.js" ;

exportar  assíncrono padrão  ( req , res ) => {    
  const  {
    nome de usuário ,
    esconder ,
    ocultar_título ,
    hide_border ,
    card_width ,
    titulo_cor ,
    text_color ,
    bg_color ,
    tema ,
    cache_seconds ,
    disposição ,
    langs_count ,
    exclude_repo ,
    custom_title ,
    localidade ,
    border_radius ,
    cor_borda ,
    disable_animations ,
  }  =  req . consulta ;
  res . setHeader ( "Tipo de conteúdo" ,  "imagem/svg+xml" ) ;

  if  ( lista negra . inclui ( nome de usuário ) )  {
    retorno  res . send ( renderError ( "Algo deu errado" ) ) ;
  }

  if  ( localidade  &&  ! isLocaleAvailable ( localidade ) )  {
    retorno  res . send ( renderError ( "Algo deu errado" ,  "Local não encontrado" ) ) ;
  }

  tente  {
    const  topLangs  =  await  fetchTopLanguages ​​(
      nome de usuário ,
      parseArray ( exclude_repo ) ,
    ) ;

    const  cacheSeconds  =  clampValue (
      parseInt ( cache_seconds  ||  CONSTANTES . FOUR_HOURS ,  10 ) ,
      CONSTANTES . QUATRO_HORAS ,
      CONSTANTES . UM_DIA ,
    ) ;

    res . setHeader (
      "Controle de Cache" ,
      `max-age= ${
        cacheSegundos  /  2
      } , s-maxage= ${ cacheSeconds } , stale-while-revalidate= ${ CONSTANTS . UM_DIA } `,
    ) ;

    retorno  res . enviar (
      renderTopLanguages ​​( topLangs ,  {
        custom_title ,
        hide_title : parseBoolean ( hide_title ) ,
        hide_border : parseBoolean ( hide_border ) ,
        card_width : parseInt ( card_width ,  10 ) ,
        ocultar : parseArray ( esconder ) ,
        titulo_cor ,
        text_color ,
        bg_color ,
        tema ,
        disposição ,
        langs_count ,
        border_radius ,
        cor_borda ,
        localidade : localidade ? localidade . toLowerCase ( ) : nulo ,
        disable_animations : parseBoolean ( disable_animations ) ,
      } ) ,
    ) ;
  }  pegar  ( err )  {
    res . setHeader ( "Cache-Control" ,  `sem cache, sem armazenamento, deve-revalidar` ) ;  // Não armazene respostas de erro em cache.
    retorno  res . send ( renderError ( err.mensagem , err.secondMessage ) ) ; _  _ _ _
  }
} ;

# Teste automatizado utilizando Mocks
  
  Mock objects são uma ótima alternativa para facilitar a escrita de testes de unidade 
  para classes que dependem de outras classes. Através de mocks podemos acesso ao banco dedos durante
  a execução dos testes.
  
# MOCKITO
    É uma API utilizada para criação de mocks.

## Comandos
   - **when(quandoForChamadoEsseMetodo()).thenReturn(deveRetornarEsseConteudo()):** informa o que será retornado quando 
     for invocado um metodo que precisa ser mockado.  
   - 
## Imports
  - org.mockito.Mockito

## Considerações
- Quando você invoca um método no mock que não foi previamente ensinado(when) a responder algo, o Mockito faz o seguinte:
  - Se o método retorna um inteiro, double, ou um tipo primitivo qualquer, ele retornará 0 
  - Se o método retorna uma lista, o Mockito retornará uma lista vazia 
  - Se o método retorna uma outra classe qualquer, o Mockito retorna null.
  

# TESTE SPRING MVC
    
    IMPORTES
    
    import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
    import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.forwardedUrl;
    import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;
    import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.view;


    @Test
    public void show404Page() throws Exception {
        mockMvc.perform(get("/error/404"))
               .andExpect(status().isOk())
               .andExpect(view().name(ErrorController.VIEW_NOT_FOUND))
               .andExpect(forwardedUrl("/WEB-INF/jsp/error/404.jsp"));
    }

    @Test
    public void showInternalServerErrorPage() throws Exception {
        mockMvc.perform(get("/error/error"))
               .andExpect(status().isOk())
               .andExpect(view().name(ErrorController.VIEW_INTERNAL_SERVER_ERROR))
               .andExpect(forwardedUrl("/WEB-INF/jsp/error/error.jsp"));
    }
    
# TESTE SPRING API REST     

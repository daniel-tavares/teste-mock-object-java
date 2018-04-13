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
  


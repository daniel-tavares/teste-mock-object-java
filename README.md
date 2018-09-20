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
    
    TESTE

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

 	@Before
	public void setUp() {
		mockEndereco = new Endereco();
		mockEndereco.setId(1l);
		mockEndereco.setRua("Nações unidas");
		mockEndereco.setCep("89765432");
		mockEndereco.setCidade("São Paulo");
		mockEndereco.setEstado("Sao Paulo");
		mockEndereco.setNumero("1000");
		mockEndereco.setComplemento("Marginal");
	}
	
	@Test
	public void shoudSaveEndereco() throws Exception {
		
		Mockito.when(enderecotService.save(mockEndereco)).thenReturn(mockEndereco);

		MockHttpServletResponse response = mockMvc.perform(MockMvcRequestBuilders
				.post("/enderecos")
				.accept(MediaType.APPLICATION_JSON)
				.content(objectMapper.writeValueAsString(mockEndereco)).characterEncoding("UTF-8")
				.contentType(MediaType.APPLICATION_JSON)).andReturn().getResponse();

		assertEquals(HttpStatus.CREATED.value(), response.getStatus());
	}
	
	
	
	
	@Test
	public void shoudSaveEnderecoSemCampoObrigatorio() throws Exception {
		
		
		mockEndereco.setRua(null);
		
		Mockito.when(enderecotService.save(mockEndereco)).thenReturn(mockEndereco);
		
		MockHttpServletResponse response = mockMvc.perform(MockMvcRequestBuilders
				.post("/enderecos")
				.accept(MediaType.APPLICATION_JSON)
				.content(objectMapper.writeValueAsString(mockEndereco)).characterEncoding("UTF-8")
				.contentType(MediaType.APPLICATION_JSON)).andReturn().getResponse();

	    assertEquals(response.getContentAsString(), "Rua é um campo brigatório");
		assertEquals(HttpStatus.BAD_REQUEST.value(), response.getStatus());
		
	}
	
	
	
	@Test
	public void shoudUpdateEndereco() throws Exception {
		
		
		mockEndereco.setRua("Rua atualizada");
		
		Mockito.when(enderecotService.save(mockEndereco)).thenReturn(mockEndereco);
		Mockito.when(enderecotService.findById(mockEndereco.getId())).thenReturn(Optional.of(mockEndereco));

		MockHttpServletResponse response = mockMvc.perform(MockMvcRequestBuilders
				.put("/enderecos")
				.accept(MediaType.APPLICATION_JSON)
				.content(objectMapper.writeValueAsString(mockEndereco)).characterEncoding("UTF-8")
				.contentType(MediaType.APPLICATION_JSON)).andReturn().getResponse();

	   assertEquals(HttpStatus.OK.value(), response.getStatus());
		
	}
	
	
	@Test
	public void shoudUpdateEnderecoSemID() throws Exception {
		
		mockEndereco.setId(null);
		mockEndereco.setRua("Rua atualizada");
		
		Mockito.when(enderecotService.save(mockEndereco)).thenReturn(mockEndereco);
		Mockito.when(enderecotService.findById(mockEndereco.getId())).thenReturn(Optional.of(mockEndereco));
		
		MockHttpServletResponse response = mockMvc.perform(MockMvcRequestBuilders
				.put("/enderecos")
				.accept(MediaType.APPLICATION_JSON)
				.content(objectMapper.writeValueAsString(mockEndereco)).characterEncoding("UTF-8")
				.contentType(MediaType.APPLICATION_JSON)).andReturn().getResponse();

	    assertEquals(response.getContentAsString(), "Campo id não pode ser nulo na atualizacao");
		assertEquals(HttpStatus.BAD_REQUEST.value(), response.getStatus());
		
	}
	
	
	
	@Test
	public void shoudDeleteEndereco() throws Exception {
		
		
		Mockito.when(enderecotService.findById(mockEndereco.getId())).thenReturn(Optional.of(mockEndereco));
		
		MockHttpServletResponse response = mockMvc.perform(MockMvcRequestBuilders
				.delete("/enderecos/1")
				.accept(MediaType.APPLICATION_JSON)
				.content(objectMapper.writeValueAsString(mockEndereco)).characterEncoding("UTF-8")
				.contentType(MediaType.APPLICATION_JSON)).andReturn().getResponse();

	    assertEquals(HttpStatus.OK.value(), response.getStatus());
		
	}
	
	
	@Test
	public void shoudDeleteEnderecoComIdInexistente() throws Exception {
		
		Mockito.when(enderecotService.findById(4l)).thenReturn(Optional.empty());
		
		MockHttpServletResponse response = mockMvc.perform(MockMvcRequestBuilders
				.delete("/enderecos/4"))
				.andReturn().getResponse();


	    assertEquals(response.getContentAsString(), "Endereco a ser atualizado não existe na base de dados");
	    assertEquals(HttpStatus.BAD_REQUEST.value(), response.getStatus());
		
	}
	
	
	@Test
	public void shoudFindEnderecoById() throws Exception {
		
		Mockito.when(enderecotService.findById(1l)).thenReturn(Optional.of(mockEndereco));
		
		MockHttpServletResponse response = mockMvc.perform(MockMvcRequestBuilders
				.get("/enderecos/1"))
				.andReturn().getResponse();

	    assertEquals(HttpStatus.OK.value(), response.getStatus());
		
	}

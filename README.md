# Prova1_OTES12

---

<h2> Como realizar a integração? </h2>
1. Baixe o package Biblioteca e o adicione no mesmo diretório da sua aplicação.
2. Na sua aplicação, faça o import - No começo do código adicione o comando "import Biblioteca.*;" 

Abaixo segue uma lista de comandos que podem ser utilizados: (estão no main.java):

		//Criar uma lista de registro das vendas:
		Registros r1 = new Registros();
				
		//Criar uma Venda / Em um caso real seria importado de outro sistema:
		System.out.println("Cadastre a Venda:");
		Vendas venda1 = new Vendas ("José", "01/2020", "Joinville", 1900);
		System.out.println("Venda Cadastrada!");

		//Adaptador agindo, transformando a Venda com parâmetro de cidade para parâmetro de Região
		//O adaptador já faz cria o registro na lista de registro de vendas
		Adapter a1 = new Adapter ("José", "01/2020", "Joinville", 1900);
		a1.CidadeRegiaoAdapterJoinville(r1);
		
		//Confere a Venda criada no Registro (BD):
		r1.getRegistrosVendas(r1);
		
		//Criando uma Cota: (ainda não cria o registro na lista de registros de cotas)
		System.out.println("Cadastre a Cota:");
		Cota cota1 = new Cota ("José", "01/2020", "Norte", 2000);
		
		//Crio o Handler - Chain of Responsability - Fará a verificação dos dados para somente após fazer o cadastro
		HandlerAuth h1 = new HandlerAuth();
		
		//Cria a data em que será cadastrada a cota
		LocalDateTime tomorrow = LocalDateTime.now().plusMonths(3);

		//Confere se o usuário pode cadastrar a cota com os Handlers
		if (h1.HandlerAuthorization("1A2B3C") == "Status Code 200") {
			//Confere se a cota está sendo cadastrada para o futuro
			if(h1.HandlerVerifyDate(tomorrow) == "Status Code 200") {
				r1.addCota(r1, "José", "01/2020", "Norte", 2000);
				System.out.println("Cota Cadastrada!");
			}
			else {
				System.out.println("Você não pode cadastrar uma cota no passado. Cota não cadastrada!");
			}
		}
		else {
			System.out.println("Seu usário não pode cadastrar uma cota. Cota não cadastrada!");
		}
		
		r1.getRegistrosCotas(r1);

---

<h2>Qual a finalidade dos componentes?</h2>

Facilitar a criação de uma aplicação de monitoramento do progresso de cada vendedor na consecução da cota.
Em relação aos meus padrões de projeto específicos (Chain of Responsibility e Adapter), eles tem a seguinte finalidade:

Adapter (classe Adapter): Faz com que objetos que tenham parâmetros imcompatíveis possam se comunicar entre si, por intermédio do Adapter. - Na minha aplicação isso foi contemplado com a adaptação do campo "Cidade" para "Região", no registro de Venda.
Chain of Responsability (classes Handlers): Fazem com que a aplicação passe os pedidos por uma corrente de processos. Cada handler (processo) decidirá se o pedido passa adiante para o próximo handler na corrente ou se é abandonado, não concluindo o processo. - Na minha aplicação isso foi contemplado fazendo verificações no momento do cadastro da Cota, conferindo se o token do usuário que estava fazendo o cadastro tem autorização e depois conferindo se a data da cota está de acordo com a possibilidade de cadastro (não é possível cadastrar cota no passado na minha aplicação).

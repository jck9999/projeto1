# projeto1
back end

cliente controler codigo.
package com.trabalho.crm.controler;


import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseStatus;
import org.springframework.web.bind.annotation.RestController;
 
import com.trabalho.crm.model.Cliente;
import com.trabalho.crm.repository.ClienteRepository;

@RestController
@RequestMapping("/clientes")
@CrossOrigin(origins="http://localhost:3000")
public class ClienteControler {

	@Autowired
	private ClienteRepository clienteRepository;
	
	
	@GetMapping
	public List<Cliente> ListarClientes() {
		return clienteRepository.findAll();
		
	}
	
	@PostMapping
	@ResponseStatus(HttpStatus.CREATED)
	public Cliente CadastrarCliente(@RequestBody Cliente cliente) {
	
		return clienteRepository.save(cliente);
		
	}
		
}

-----------------------------------------------------------------------------------------------------------
cliente.java
package com.trabalho.crm.model;

import java.math.BigDecimal;
import java.util.Date;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

import com.fasterxml.jackson.annotation.JsonFormat;

import lombok.Data;

@Data
@Entity
public class Cliente {
   
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	public Long id;
	
	
	@Column(nullable = false )
	public String nome;
	
	@Column(nullable = false )
	public String cpf;
	
	@Column(nullable = false )
	@JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd")
	public Date data_nascimento;
	
	@Column(nullable = false )
	@JsonFormat(shape = JsonFormat.Shape.STRING)
	public int fone;
	 
	@JsonFormat(shape = JsonFormat.Shape.STRING)
	public BigDecimal limite_credito;
	
	
	public Long getId() {
		return id;
	}
//
//
//	public void setId(Long id) {
//		this.id = id;
//	}
//
//
//	public String getNome() {
//		return nome;
//	}
//	 
//	public void setNome(String novoNome) {
//		this.nome = novoNome;
//	}
//	
//	
//	public String getCpf() {
//        return cpf;
//	}
//	
//	public void setCpf(String novoCpf) {
//		this.cpf = novoCpf;
//	}
//	
//	
//	public Date getDatadenascimento() {
//		return this.Datadenascimento;
//	}
//
//
//	public void setDatadenascimento(Date datadenascimento) {
//		this.Datadenascimento = datadenascimento;
//	}
//
//
//	public int getFone() {
//		return this.fone;
//	}
//
//
//	public void setFone(int fone) {
//		this.fone = fone;
//	}
//	
//	
//
//	public BigDecimal getLimiteDeCredito() {
//		 return this.LimiteDeCredito;
//	}
//
//	public void setLimiteDeCredito(BigDecimal novoLimite) {
//		this.LimiteDeCredito = novoLimite;
//	}
//
////	@Override
////	public int hashCode() {
////		final int prime = 31;
////		int result = 1;
////		result = prime * result + ((id == null) ? 0 : id.hashCode());
////		return result;
////	}
////
////
////	@Override
////	public boolean equals(Object obj) {
////		if (this == obj)
////			return true;
////		if (obj == null)
////			return false;
////		if (getClass() != obj.getClass())
////			return false;
////		Cliente other = (Cliente) obj;
////		if (id == null) {
////			if (other.id != null)
////				return false;
////		} else if (!id.equals(other.id))
////			return false;
////		return true;
////	}
//
//
//	
//	
//	
//	
//	
//	
}
----------------------------------------------------------------------------------------------------------------------------------------------
clienteRepository
package com.trabalho.crm.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import com.trabalho.crm.model.Cliente;

@Repository
public interface ClienteRepository extends JpaRepository<Cliente, Long> {

}
---------------------------------------------------------------------------------
corsfiltre
package com.trabalho.crm.config;
import org.springframework.core.Ordered;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;


@Component
@Order(Ordered.HIGHEST_PRECEDENCE)
public class CorsFilter implements Filter {

    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain)
            throws IOException, ServletException {
        HttpServletResponse response = (HttpServletResponse) res;
        HttpServletRequest request = (HttpServletRequest) req;

        response.setHeader("Access-Control-Allow-Origin", "*");
        response.setHeader("Access-Control-Allow-Credentials", "true");
        response.setHeader("Access-Control-Allow-Methods",
                "ACL, CANCELUPLOAD, CHECKIN, CHECKOUT, COPY, DELETE, GET, HEAD, LOCK, MKCALENDAR, MKCOL, MOVE, OPTIONS, POST, PROPFIND, PROPPATCH, PUT, REPORT, SEARCH, UNCHECKOUT, UNLOCK, UPDATE, VERSION-CONTROL");
        response.setHeader("Access-Control-Max-Age", "3600");
        response.setHeader("Access-Control-Allow-Headers",
                "Origin, X-Requested-With, Content-Type, Accept, Key, Authorization");
        if ("OPTIONS".equalsIgnoreCase(request.getMethod())) {
            response.setStatus(HttpServletResponse.SC_OK);
        } else {
            chain.doFilter(req, res);
        }
    }

    public void init(FilterConfig filterConfig) {
        // not needed
    }

    public void destroy() {
        // not needed
    }

}
----------------------------------------------------------------------------------------------------------------------
aplication.java
package com.trabalho.crm;

//import org.springframework.boot.SpringApplication;
//import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;

@SpringBootApplication
@EnableAutoConfiguration
public class Application {

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}

}
--------------------------------------------------------------------------
front end-js
--------------------------------------------------------------------------
input.js
import React,{useEffect, useRef}  from 'react';
import{useField} from '@unform/core';

export default function Input({name, ... rest}){
    const inputRef = useRef(null);
    const {fieldName,registerField,defaultValue,error} = useField(name);

    useEffect (() => {

        registerField({
            name: fieldName,
            ref: inputRef.current,
            path: 'value'
        })
    },[fieldName,registerField]);


    return(
        <div>


        
        <input ref={inputRef} defaultValue={defaultValue} {... rest} />
        
        {error && <span style={{color:'#f00'}}>{error}</span>}
        </div>


    );
}
---------------------------------------------------------------------------
app.js
import React, {useRef, useState} from 'react';

import './App.css';

import axios from 'axios';

function App() {
	const formRef = useRef(null);
	const api = axios.create({
		baseURL: 'http://localhost:8080'	
	});
	
  	function handleSubmit(data,{ reset }){
	  	if (data.nome  === ""){
			formRef.current.setErrors({
				nome:'o nome e obrigatorio'
			});	
	  	}
		
  		console.log(data); 
		api.post('clientes',data.id).then(reset());
	
  	}

	  

  return (
    <div className="App">
      <h1>Cadastrar Clientes</h1>


      <form ref={formRef}  onSubmit = {handleSubmit}>
        <input name="nome" />
        <input name="cpf" />
        <input name="data_nascimento" type = "date" />
        <input name="fone" type = "int" />
		<input name="limite_credito" type = "bigdecimal" />

        <button type="submit" >enviar</button>
      </form>

	</div>
  );
}

export default App;
-------------------------------------------------------------------------------
index.js
void
------------------------------------------------------------------------------
app.css
.App {
  text-align: center;
}

.App-logo {
  height: 40vmin;
  pointer-events: none;
}

@media (prefers-reduced-motion: no-preference) {
  .App-logo {
    animation: App-logo-spin infinite 20s linear;
  }
}

.App-header {
  background-color: #282c34;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: calc(10px + 2vmin);
  color: white;
}

.App-link {
  color: #61dafb;
}

@keyframes App-logo-spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}


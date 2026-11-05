# Diagrama de classes: Urna Cidadã

```mermaid

classDiagram
direction LR
    class Urna {
	    -EstadoUrna estado
	    -Cargo cargoAtual
	    -List~Cargo~ sequenciaCargos
	    -RepositorioVotos repositorioVotos
	    +iniciarSessao() void
	    +finalizarSessao() void
	    +registrarVoto(voto: Voto) void
	    +obterCargoAtual() Cargo
	    +proximoCargo() void
	    +obterEstado() EstadoUrna
    }

    class EstadoUrna {
	    PARADA
	    VOTANDO
	    FINALIZADA
    }

    class Cargo {
	    VEREADOR
	    PREFEITO
    }

    class ControladorVotacao {
	    -Urna urna
	    -RepositorioCandidatos repositorioCandidatos
	    -String numeroDigitado
	    -Candidato candidatoSelecionado
	    +digitarNumero(numero: String) void
	    +corrigirNumero() void
	    +buscarCandidato() Candidato
	    +votarEmBranco() void
	    +confirmarVoto() void
    }

    class Candidato {
	    -String nome
	    -String numero
	    -Partido partido
	    -String foto
	    +validarNumero() boolean*
	    +getTipo() String*
    }

    class CandidatoProporcional {
	    +validarNumero() boolean
	    +getTipo() String
    }

    class CandidatoMajoritario {
	    +validarNumero() boolean
	    +getTipo() String
    }

    class Partido {
	    -int numero
	    -String nome
	    -String sigla
    }

    class Voto {
	    -Cargo cargo
	    -LocalDateTime timestamp
	    +getTipo() String*
    }

    class VotoCandidato {
	    -Candidato candidato
	    +getTipo() String
	    +obterCandidato() Candidato
    }

    class VotoBranco {
	    +getTipo() String
    }

    class VotoNulo {
	    +getTipo() String
    }

    class RepositorioVotos {
	    +salvar(voto: Voto) void
	    +obterTodos() List~Voto~
	    +obterPorCargo(cargo: Cargo) List~Voto~
	    +limpar() void
    }

    class FirebaseRepositorio {
	    +salvar(voto: Voto) void
	    +obterTodos() List~Voto~
	    +obterPorCargo(cargo: Cargo) List~Voto~
	    +limpar() void
    }

    class MemoriaRepositorio {
	    +salvar(voto: Voto) void
	    +obterTodos() List~Voto~
	    +obterPorCargo(cargo: Cargo) List~Voto~
	    +limpar() void
    }

    class RepositorioCandidatos {
	    +buscarPorNumero(numero: String, cargo: Cargo) Candidato
	    +obterTodos(cargo: Cargo) List~Candidato~
    }

    class FirebaseRepositorioCandidatos {
	    +buscarPorNumero(numero: String, cargo: Cargo) Candidato
	    +obterTodos(cargo: Cargo) List~Candidato~
    }

    class MemoriaRepositorioCandidatos {
	    +buscarPorNumero(numero: String, cargo: Cargo) Candidato
	    +obterTodos(cargo: Cargo) List~Candidato~
    }

    class Apuracao {
	    -RepositorioVotos repositorioVotos
	    +obterContagem(cargo: Cargo) Map~String, Integer~
	    +obterVotosEmBranco(cargo: Cargo) int
	    +obterVotosNulos(cargo: Cargo) int
	    +obterResultado(cargo: Cargo) ResultadoApuracao
    }

    class ResultadoApuracao {
	    -Cargo cargo
	    -Map~String, Integer~ votos
	    -int votosEmBranco
	    -int votosNulos
	    -int totalVotos
    }

	<<enumeration>> EstadoUrna
	<<enumeration>> Cargo
	<<abstract>> Candidato
	<<abstract>> Voto
	<<interface>> RepositorioVotos
	<<interface>> RepositorioCandidatos

    Candidato <|-- CandidatoProporcional
    Candidato <|-- CandidatoMajoritario
    Voto <|-- VotoCandidato
    Voto <|-- VotoBranco
    Voto <|-- VotoNulo
    RepositorioVotos <|.. FirebaseRepositorio
    RepositorioVotos <|.. MemoriaRepositorio
    RepositorioCandidatos <|.. FirebaseRepositorioCandidatos
    RepositorioCandidatos <|.. MemoriaRepositorioCandidatos
    Urna --> RepositorioVotos : utiliza
    Urna --> Cargo : controla
    Urna --> EstadoUrna : possui
    ControladorVotacao --> Urna : controla
    ControladorVotacao --> RepositorioCandidatos : consulta
    ControladorVotacao --> Candidato : seleciona
    Candidato --> Partido : pertence a
    VotoCandidato --> Candidato : registra
    Voto --> Cargo : pertence a
    Apuracao --> RepositorioVotos : consulta
    Apuracao --> ResultadoApuracao : gera

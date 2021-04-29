# AV1exercicio01
Exercicio 01 da AV 1
import java.util.ArrayList;
import java.util.List;
import java.util.function.Function;

public class Applicacao {

    public static void main(String[] args) {
        aplicacao programa = new aplicacao();
        programa.executar();
    }

    public void executar() {
        List<Paciente> pacientes = new ArrayList<>();
        Paciente paciente = null;
        while ((paciente = lerPaciente()) != null) {
            pacientes.add(paciente);
        }
        IO.println("Quantidade de pacientes: %d%n", quantidade(pacientes))
        IO.printf("Quantidade de pessoas com idade entre 18 e 25: %d%n", quantidade(pacientes, pessoa -> pessoa.idade >= 18 && pessoa.idade <= 25));
        IO.printf("Nome do paciente mais velho: %s%n", maior(pacientes, pessoa -> pessoa.idade).nome);

    private Paciente lerPaciente() {
        String nome = IO.readString("Nome: ", texto -> !texto.trim().isEmpty());
        if ("fim".equalsIgnoreCase(nome)) {
            return null;
        }
        Paciente paciente = new Paciente();
        paciente.nome = nome;
        paciente.sexo = IO.readChar("Sexo: ", letra -> letra == 'm' || letra == 'f');
        paciente.peso = IO.readDouble("Peso: ", numero -> numero > 0);
        paciente.idade = IO.readInt("Idade: ", numero -> numero > 0);
        paciente.altura = IO.readDouble("Altura: ", numero -> numero > 0);
        return paciente;
    }

    private Paciente maior(List<Paciente> pacientes, Function<Paciente, Integer> funcao) {
        Paciente maior = pacientes.get(0);
        for (int i = 1; i < pacientes.size(); i++) {
            Paciente paciente = pacientes.get(i);
            if (funcao.apply(paciente) > funcao.apply(maior)) {
                maior = paciente;
            }
        }
        return maior;
    }

    private Object media(List<Paciente> pacientes, Predicate<Paciente> condicao) {
        int soma = 0;
        for (Paciente paciente : pacientes) {
            if (condicao.test(paciente)) {
                soma += paciente.idade;
            }
        }
        return soma / pacientes.size();
    }

    private Paciente menor(List<Paciente> pacientes, Function<Paciente, Double> funcao, Predicate<Paciente> condicao) {
        Paciente menor = null;
        for (Paciente paciente : pacientes) {
            if (condicao.test(paciente)) {
                if (menor == null) {
                    menor = paciente;
                }
                if (funcao.apply(paciente) < funcao.apply(menor)) {
                    menor = paciente;
                }
            }
        }
        return menor;
    }

    private int quantidade(List<Paciente> pacientes) {
        return pacientes.size();
    }

    private int quantidade(List<Paciente> pacientes, Predicate<Paciente> condicao) {
        int quantidade = 0;
        for (Paciente paciente : pacientes) {
            if (condicao.test(paciente)) {
                quantidade++;
            }
        }
        return quantidade;
    }
}

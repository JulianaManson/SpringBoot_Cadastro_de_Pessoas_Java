import jakarta.persistence.*;

@Entity
public class Pessoa {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nome;
    private String email;
    private String telefone;

    // Getters e Setters
}
import org.springframework.data.jpa.repository.JpaRepository;

public interface PessoaRepository extends JpaRepository<Pessoa, Long> {
}
import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class PessoaService {
    private final PessoaRepository repository;

    public PessoaService(PessoaRepository repository) {
        this.repository = repository;
    }

    public List<Pessoa> listarTodos() {
        return repository.findAll();
    }

    public Pessoa salvar(Pessoa pessoa) {
        return repository.save(pessoa);
    }

    public void deletar(Long id) {
        repository.deleteById(id);
    }
}
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/pessoas")
public class PessoaController {
    private final PessoaService service;

    public PessoaController(PessoaService service) {
        this.service = service;
    }

    @GetMapping
    public List<Pessoa> listar() {
        return service.listarTodos();
    }

    @PostMapping
    public Pessoa adicionar(@RequestBody Pessoa pessoa) {
        return service.salvar(pessoa);
    }

    @DeleteMapping("/{id}")
    public void remover(@PathVariable Long id) {
        service.deletar(id);
    }
}
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class CadastroApplication {
    public static void main(String[] args) {
        SpringApplication.run(CadastroApplication.class, args);
    }
}


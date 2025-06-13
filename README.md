import java.util.ArrayList;

interface ItemBiblioteca {
    void realizarEmprestimo();
    void realizarDevolucao();
    void exibirInformacoes();
}

abstract class Livro implements ItemBiblioteca {
    private String titulo;
    private String autor;
    private int anoPublicacao;
    protected boolean disponivel;

    public Livro(String titulo, String autor, int anoPublicacao) {
        this.titulo = titulo;
        this.autor = autor;
        this.anoPublicacao = anoPublicacao;
        this.disponivel = true;
    }

    public String getTitulo() {
        return titulo;
    }


    public String getAutor() {
        return autor;
    }

    public int getAnoPublicacao() {
        return anoPublicacao;
    }

    public boolean isDisponivel() {
        return disponivel;
    }

    public void setDisponivel(boolean disponivel) {
        this.disponivel = disponivel;
    }

    public void emprestar() {
        if (this.disponivel) {
            System.out.println("Livro emprestado com sucesso");
            this.disponivel = false;
        } else {
            System.out.println("Livro indisponível para empréstimo");
        }
    }

    public void devolver() {
        if (!this.disponivel) {
            System.out.println("Livro devolvido com sucesso");
            this.disponivel = true;
        } else {
            System.out.println("Livro já está disponível");
        }
    }

    @Override
    public void realizarEmprestimo() {
        emprestar();
    }

    @Override
    public void realizarDevolucao() {
        devolver();
    }

    public abstract void exibirDetalhes();
}


class LivroFisico extends Livro {
    private String localizacaoPrateleira;

    public LivroFisico(String titulo, String autor, int anoPublicacao, String localizacaoPrateleira) {
        super(titulo, autor, anoPublicacao);
        this.localizacaoPrateleira = localizacaoPrateleira;
    }

    @Override
    public void exibirDetalhes() {
        System.out.println("Título: " + getTitulo());
        System.out.println("Autor: " + getAutor());
        System.out.println("Ano de publicação: " + getAnoPublicacao());
        System.out.println("Disponível: " + (isDisponivel() ? "Sim" : "Não"));
        System.out.println("Localização na prateleira: " + localizacaoPrateleira);
    }

    @Override
    public void exibirInformacoes() {
        exibirDetalhes();
    }
}

class Ebook extends Livro {
    private String formato;
    private double tamanhoArquivo;

    public Ebook(String titulo, String autor, int anoPublicacao, String formato, double tamanhoArquivo) {
        super(titulo, autor, anoPublicacao);
        this.formato = formato;
        this.tamanhoArquivo = tamanhoArquivo;
    }

    @Override
    public void exibirDetalhes() {
        System.out.println("Título: " + getTitulo());
        System.out.println("Autor: " + getAutor());
        System.out.println("Ano de publicação: " + getAnoPublicacao());
        System.out.println("Disponível: " + (isDisponivel() ? "Sim" : "Não"));
        System.out.println("Formato: " + formato);
        System.out.println("Tamanho do arquivo: " + tamanhoArquivo + "MB");
    }

    @Override
    public void exibirInformacoes() {
        exibirDetalhes();
    }
}
class Usuario {
    private String nome;
    private String idUsuario;
    private ArrayList<Livro> livrosEmprestados = new ArrayList<Livro>();

    public Usuario(String nome, String idUsuario) {
        this.nome = nome;
        this.idUsuario = idUsuario;
    }

    public String getNome() {
        return nome;
    }

    public String getIdUsuario() {
        return idUsuario;
    }

    public ArrayList<Livro> getLivrosEmprestados() {
        return livrosEmprestados;
    }

    public void emprestarLivro(Livro livro) {
        if (livro.isDisponivel()) {
            livro.emprestar();
            livrosEmprestados.add(livro);
            System.out.println("Livro emprestado com sucesso");
        } else {
            System.out.println("Livro indisponível para empréstimo");
        }
    }

    public void devolverLivro(Livro livro) {
        if (livrosEmprestados.contains(livro)) {
            livro.devolver();
            livrosEmprestados.remove(livro);
            System.out.println("Livro devolvido com sucesso");
        } else {
            System.out.println("Livro não está emprestado para este usuário");
        }
    }

    public void exibirLivrosEmprestados() {
        if (livrosEmprestados.isEmpty()) {
            System.out.println("Nenhum livro emprestado");
        } else {
            System.out.println("Livros emprestados:");
            for (Livro livro : livrosEmprestados) {
                livro.exibirDetalhes();
            }
        }
    }
}

// Classe Biblioteca
class Biblioteca {
    private String nome;
    private ArrayList<ItemBiblioteca> catalogoDeItens = new ArrayList<>();
    private ArrayList<Usuario> usuariosRegistrados = new ArrayList<>();

    public Biblioteca(String nome) {
        this.nome = nome;
    }

    public String getNome() {
        return nome;
    }

    public void adicionarItem(ItemBiblioteca item) {
        catalogoDeItens.add(item);
    }

    public void registrarUsuario(Usuario usuario) {
        usuariosRegistrados.add(usuario);
    }

    public void exibirCatalogo() {
        System.out.println("Catálogo da Biblioteca " + nome + ":");
        for (ItemBiblioteca item : catalogoDeItens) {
            item.exibirInformacoes();
            System.out.println("--------------------");
        }
    }

    public void exibirUsuarios() {
        System.out.println("Usuários registrados na Biblioteca " + nome + ":");
        for (Usuario usuario : usuariosRegistrados) {
            System.out.println("Nome: " + usuario.getNome() + ", ID: " + usuario.getIdUsuario());
        }
    }
    public void emprestarLivro(Usuario usuario, Livro livro) {
        if (usuariosRegistrados.contains(usuario)) {
            usuario.emprestarLivro(livro);
        } else {
            System.out.println("Usuário não registrado na biblioteca");
        }
    }
    public void devolverLivro(Usuario usuario, Livro livro) {
        if (usuariosRegistrados.contains(usuario)) {
            usuario.devolverLivro(livro);
        } else {
            System.out.println("Usuário não registrado na biblioteca");
        }
    }
    public static void main(String[] args) {
        Biblioteca biblioteca = new Biblioteca("Biblioteca Central");

        LivroFisico livro1 = new LivroFisico("Dom Casmurro", "Machado de Assis", 1899, "Prateleira A1");
        Ebook ebook1 = new Ebook("1984", "George Orwell", 1949, "PDF", 1.5);

        biblioteca.adicionarItem(livro1);
        biblioteca.adicionarItem(ebook1);

        Usuario usuario1 = new Usuario("João Silva", "12345");
        biblioteca.registrarUsuario(usuario1);

        biblioteca.exibirCatalogo();
        biblioteca.exibirUsuarios();

        biblioteca.emprestarLivro(usuario1, livro1);
        usuario1.exibirLivrosEmprestados();

        biblioteca.devolverLivro(usuario1, livro1);
        usuario1.exibirLivrosEmprestados();
    }
}


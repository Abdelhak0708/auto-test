@Component
public class DataInitializer implements CommandLineRunner {

    @Autowired
    private DocumentRepository documentRepository;

    @Override
    public void run(String... args) throws Exception {
        ObjectMapper mapper = new ObjectMapper();

        TypeReference<List<Document>> typeRef = new TypeReference<List<Document>>() {};
        InputStream inputStream = getClass().getResourceAsStream("/data.json");

        List<Document> documents = mapper.readValue(inputStream, typeRef);
        documentRepository.saveAll(documents);

        System.out.println("✅ Données initiales chargées avec succès !");
    }
}

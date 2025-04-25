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


@Service
public class AppVersionChecker {

    private final RestTemplate restTemplate = new RestTemplate();

    public List<AppInfo> checkAppsVersions(List<String> urls) {
        List<AppInfo> result = new ArrayList<>();

        for (String url : urls) {
            try {
                // Exemple : on suppose que l'endpoint "/" renvoie un JSON contenant "name" et "version"
                ResponseEntity<Map> response = restTemplate.getForEntity(url, Map.class);
                Map<String, Object> body = response.getBody();

                if (body != null) {
                    String name = (String) body.get("name");
                    String version = (String) body.get("version");

                    result.add(new AppInfo(name, version));
                }

            } catch (Exception e) {
                System.out.println("Erreur lors de la récupération depuis " + url + ": " + e.getMessage());
            }
        }

        return result;
    }
}

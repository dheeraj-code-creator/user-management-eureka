# user-management-eureka
============================================================================================================
<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.0.0.BUILD-SNAPSHOT</version>
		<relativePath />
		<!-- lookup parent from repository -->
	</parent>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
		<elastic.version>7.3.1</elastic.version>
	</properties>
	
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>

		<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-lang3 -->
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-lang3</artifactId>
			<version>3.10</version>
		</dependency>
		
		<dependency>
			<groupId>org.elasticsearch.client</groupId>
			<artifactId>elasticsearch-rest-high-level-client</artifactId>
			<version>${elastic.version}</version>
		</dependency>
		<dependency>
			<groupId>org.elasticsearch.client</groupId>
			<artifactId>elasticsearch-rest-client</artifactId>
			<version>${elastic.version}</version>
		</dependency>
		 <dependency>
			<groupId>org.elasticsearch</groupId>
			<artifactId>elasticsearch</artifactId>
			<version>${elastic.version}</version><!--$NO-MVN-MAN-VER$-->
		</dependency>
		
	</dependencies>
	
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
	
</project>

==========================================================================================

public class User {
	
	private String userId;
	private String userName;
	
	public User() {
		super();
	}
	public User(String userId, String userName) {
		super();
		this.userId = userId;
		this.userName = userName;
	}
	public String getUserId() {
		return userId;
	}
	public void setUserId(String userId) {
		this.userId = userId;
	}
	public String getUserName() {
		return userName;
	}
	public void setUserName(String userName) {
		this.userName = userName;
	}
	

}
===============================================================================

@RestController
@RequestMapping(value = "/userinfo")
public class UserController {
	
	@Autowired
	private UserService userService;
	
	@GetMapping(value = "/alluser", produces = MediaType.APPLICATION_JSON_VALUE)
	public List<User> getAllUserInfo(){
		return userService.getAllUser();
	}

}

==================================================================================

@Service
public class UserService {
	
	@Autowired
	private UserRepository userRepository;

	public List<User> getAllUser() {
		return userRepository.findAllUserDetailsFromElastic();
	}

}

===================================================================================

@Component
public interface UserRepository {

	List<User> findAllUserDetailsFromElastic();

}
===================================================================================

@Component
public class UserRepositoryImpl implements UserRepository{
	
	@Autowired
	private RestHighLevelClient client;

	@Override
	public List<User> findAllUserDetailsFromElastic() {
		SearchRequest searchRequest = new SearchRequest();
		searchRequest.indices("airinfo");
		SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder();
		searchSourceBuilder.query(buildCustomerSearchQuery());
		SearchResponse searchResponse = null;
		searchResponse = client.get(searchRequest, RequestOptions.DEFAULT);
	//	List<User> userList = new ArrayList<>();
	//	userList = searchResponse.getHits()
		return null;
	}

	private QueryBuilder buildCustomerSearchQuery() {
		BoolQueryBuilder boolQueryBuilder = QueryBuilders.boolQuery();
		return null;
	}

}

=================================================================================

server.servlet.context-path=/user-management
server.port = 9091

=================================================================================

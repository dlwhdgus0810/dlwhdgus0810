# Hyun Lee

### 🏗️ Architecture
```text
├─ src/main
│   ├─ kotlin/me.hyunlee.laundry.profile
│   │   ├─ adapter
│   │   │   ├─ in (Web Adapters)
│   │   │   └─ out (Infrastructure Adapters)
│   │   ├─ application
│   │   │   ├─ port (Inbound/Outbound Ports)
│   │   │   └─ service (Use Case Implementations)
│   │   └─ domain
│   │       └─ model (Core Domain Models)
│   └─ resources
│       └─ application.yml
```
## ⚙️ Configuration (application.yml)

```yaml
profile:
  name: "Hyun Lee"
  education: "University of Virginia"
  major: "Computer Science"
  focus: "Backend Development"
  portfolioUrl: "https://hyunlee.me"
  contactEmail: "hyunlee.289@gmail.com"
  linkedInUrl: "https://www.linkedin.com/in/jleee/"

info:
  app:
    name: hyunlee-profile-service
    version: 2.3.2
    description: "Personal profile as code"
  contact:
    email: hyunlee.289@gmail.com
    github: github.com/dlwhdgus0810
    linkedin: linkedin.com/in/jleee
```
## 📦 Infrastructure Layer: Tech Stack & Config
```kotlin
@Configuration
@EnableConfigurationProperties(ProfileProperties::class)
class ProfileConfig

@ConfigurationProperties(prefix = "profile")
data class ProfileProperties(
    val name: String = "Hyun Lee",
    val education: String? = "University of Virginia",
    val major: String? = "Computer Science",
    val focus: String? = "Backend Development",
    val portfolioUrl: String? = "https://hyunlee.me",
    val contactEmail: String? = "hyunlee.289@gmail.com",
    val linkedInUrl: String? = "https://www.linkedin.com/in/jleee/"
) {
    fun toDomain() = Profile(
        name = name,
        education = education ?: "N/A",
        major = major ?: "N/A",
        focus = focus ?: "N/A",
        portfolioUrl = portfolioUrl ?: "",
        contactEmail = contactEmail ?: "",
        linkedInUrl = linkedInUrl ?: ""
    )
}
```

<pre>
<b>@Component</b>
<b>class</b> TechStackProvider {

    <i>/**
     * Categorized tech stack badges
     */</i>
    <b>val</b> categories = <b>listOf</b>(
        Category.BACKEND <b>to listOf</b>(
            Badge("Java", <img src="https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=java&logoColor=white" height="20">),
            Badge("Spring", <img src="https://img.shields.io/badge/Spring-6DB33F?style=flat-square&logo=spring&logoColor=white" height="20">),
            Badge("Spring Boot", <img src="https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat-square&logo=spring-boot&logoColor=white" height="20">),
            Badge("Spring Data JPA", <img src="https://img.shields.io/badge/Spring%20Data%20JPA-6DB33F?style=flat-square&logo=spring&logoColor=white" height="20">)
        ),
        Category.DATABASE <b>to listOf</b>(
            Badge("MySQL", <img src="https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white" height="20">),
            Badge("Redis", <img src="https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white" height="20">)
        ),
        Category.CLOUD <b>to listOf</b>(
            Badge("AWS", <img src="https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazon-aws&logoColor=white" height="20">),
            Badge("EC2", <img src="https://img.shields.io/badge/EC2-FF9900?style=flat-square&logo=amazon-ec2&logoColor=white" height="20">),
            Badge("Amazon RDS", <img src="https://img.shields.io/badge/Amazon%20RDS-527FFF?style=flat-square&logo=amazon-rds&logoColor=white" height="20">),
            Badge("S3", <img src="https://img.shields.io/badge/S3-569A31?style=flat-square&logo=amazon-s3&logoColor=white" height="20">),
            Badge("Lambda", <img src="https://img.shields.io/badge/AWS%20Lambda-FF9900?style=flat-square&logo=amazon-aws&logoColor=white" height="20">)
        ),
        Category.DEVOPS <b>to listOf</b>(
            Badge("Docker", <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white" height="20">),
            Badge("Nginx", <img src="https://img.shields.io/badge/Nginx-009639?style=flat-square&logo=nginx&logoColor=white" height="20">)
        )
    )
}
</pre>

## 🛠️ Domain Layer

```kotlin
data class Profile(
    val name: String,
    val education: String,
    val major: String,
    val focus: String,
    val portfolioUrl: String,
    val contactEmail: String,
    val linkedInUrl: String
)

enum class Category {
    BACKEND, DATABASE, CLOUD, DEVOPS
}

data class Badge(val label: String, val url: String)
```

## 🚀 Application Layer (Ports & Services)

```kotlin
// --- Inbound Port ---
interface GetProfileUseCase {
    fun getProfile(): Profile
    fun getTechStack(): List<Pair<Category, List<Badge>>>
}

// --- Service Implementation ---
@Service
class ProfileService(
    private val profileProp: ProfileProperties,
    private val techStackProvider: TechStackProvider
) : GetProfileUseCase {
    override fun getProfile() = profileProp.toDomain()
    override fun getTechStack() = techStackProvider.categories
}
```

## 🔌 Adapter Layer (Inbound Web)

```kotlin
@RestController
@RequestMapping("/api/v1/profile")
class ProfileController(
    private val getProfileUseCase: GetProfileUseCase
) {
    @GetMapping
    fun getProfile() = ResponseEntity.ok(
        mapOf(
            "user" to getProfileUseCase.getProfile(),
            "stack" to getProfileUseCase.getTechStack()
        )
    )
}
```


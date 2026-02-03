# GitHub Profile — Hyun Lee

### 🏗️ Project Structure
```text
├─ domain
│   ├─ Profile
│   ├─ Category
│   └─ TechCategory
│
├─ modules.techstack
│   └─ TechStackModule
│
└─ application
    └─ ProfileFacade
```

## 🛠️ Domain

```kotlin
data class Profile(
    val name: String = "Hyun Lee",
    val education: String = "University of Virginia",
    val major: String = "Computer Science",
    val focus: String = "Backend Development",
    val portfolioUrl: String = "https://hyunlee.me",
    val contactEmail: String = "hyunlee.289@gmail.com"
)

enum class Category {
    BACKEND, DATABASE, CLOUD, DEVOPS
}

data class Badge(
    val label: String,
    val url: String
)

data class TechCategory(
    val category: Category,
    val badges: List<Badge>
)
```

## 📦 Modules Tech Stack

<pre>
<b>object</b> TechStackModule {

    <i>/**
     * Module boundary:
     * - Owns tech stack representation
     * - Exposes read-only data to the application layer
     */</i>
    <b>object</b> TechStack {

        <b>val</b> categories: List&lt;TechCategory&gt; = <b>listOf</b>(
            TechCategory(
                category = Category.BACKEND,
                badges = <b>listOf</b>(
                    Badge("Java", <img src="https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=java&logoColor=white" height="20">),
                    Badge("Spring", <img src="https://img.shields.io/badge/Spring-6DB33F?style=flat-square&logo=spring&logoColor=white" height="20">),
                    Badge("Spring Boot", <img src="https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat-square&logo=spring-boot&logoColor=white" height="20">),
                    Badge("Spring Data JPA", <img src="https://img.shields.io/badge/Spring%20Data%20JPA-6DB33F?style=flat-square&logo=spring&logoColor=white" height="20">)
                )
            ),
            TechCategory(
                category = Category.DATABASE,
                badges = <b>listOf</b>(
                    Badge("MySQL", <img src="https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white" height="20">),
                    Badge("Redis", <img src="https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white" height="20">)
                )
            ),
            TechCategory(
                category = Category.CLOUD,
                badges = <b>listOf</b>(
                    Badge("AWS", <img src="https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazon-aws&logoColor=white" height="20">),
                    Badge("EC2", <img src="https://img.shields.io/badge/EC2-FF9900?style=flat-square&logo=amazon-ec2&logoColor=white" height="20">),
                    Badge("Amazon RDS", <img src="https://img.shields.io/badge/Amazon%20RDS-527FFF?style=flat-square&logo=amazon-rds&logoColor=white" height="20">),
                    Badge("S3", <img src="https://img.shields.io/badge/S3-569A31?style=flat-square&logo=amazon-s3&logoColor=white" height="20">),
                    Badge("Lambda", <img src="https://img.shields.io/badge/AWS%20Lambda-FF9900?style=flat-square&logo=amazon-aws&logoColor=white" height="20">)
                )
            ),
            TechCategory(
                category = Category.DEVOPS,
                badges = <b>listOf</b>(
                    Badge("Docker", <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white" height="20">),
                    Badge("Nginx", <img src="https://img.shields.io/badge/Nginx-009639?style=flat-square&logo=nginx&logoColor=white" height="20">)
                )
            )
        )
    }
}
</pre>

## 🚀 Application (Facade)

```kotlin
class ProfileFacade(
    private val profile: Profile = Profile()
) {
    fun summary(): String =
        "${profile.name} | ${profile.major} | ${profile.focus}"
}
```

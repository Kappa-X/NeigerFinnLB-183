# Applikationssicherheit verstehen und durch Implementation garantieren.

### Einleitung
In diesem Portfolio werde ich die erreichten Handlungsziele im Modul 183 nachweisen. Dafür werde ich für jedes Handlungsziel einen eigenen Abschnitt erstellen, in welchem ich dieses behandle. Der Nachweis für das Erreichen des Handlungsziels soll in einem Abschnitt durch ein Artefakt, welches mit dem Modul zusammenhängt, eine Beschreibung dieses Artefakts und die Beurteilung der Umsetzung des Artefakts im Hinblick auf das jeweilige Handlungsziel erfolgen.

| Handlungsziel | Beschreibung | Gewichtung |
| ----------- | ----------- | ----------- |
| 1   | Aktuelle Bedrohungen erkennen und erläutern können. Aktuelle Informationen zum Thema (Erkennung und Gegenmassnahmen) beschaffen und mögliche Auswirkungen aufzeigen und erklären können. | 10% |
| 2   | Sicherheitslücken und ihre Ursachen in einer Applikation erkennen können. Gegenmassnahmen vorschlagen und implementieren können. | 30% |
| 3   | Mechanismen für die Authentifizierung und Autorisierung umsetzen können. | 20% |
| 4   | Sicherheitsrelevante Aspekte bei Entwurf, Implementierung und Inbetriebnahme berücksichtigen. | 30% |
| 5   | Informationen für Auditing und Logging generieren. Auswertungen und Alarme definieren und implementieren. | 10% |

## Handlungsziel 1

### Artefakt
![Artefakt des Handlungsziel 1](https://cdn.discordapp.com/attachments/912953190251642910/1184432122904190996/Screenshot_2023-12-13_104911.png?ex=658bf35f&is=65797e5f&hm=4e5a7c1074d40f8eb6ea0ae60fdc23ebd5ddb7e9d4c50f15c5dfaa9799fb3d91&)

### Wie wurde das Handlungsziel mit dem Artefakt erreicht?
Diese Tabelle zeigt eine selbsterstellte Zusammenfassung der OWASP Top Ten Application Security Risks des Jahres 2021, welches den ersten Teil des Handlungsziel betrifft. Die Zusammenfassung gibt einem den Überblick der aktuell 10 grössten Bedrohungen für Webanwendungssicherheit. Die Erkennung der Bedrohungen wird durch die Darstellung von Fakten/ Zahlen zu Testabdeckungen in den verschiedenen Kategorien (z. B. 94% der Anwendungen getestet für Broken Access Control) nachgewiesen. Auch werden Gegenmassnahmen für diese Bedrohungen aufgezeigt, womit der zweite Teil des Handlungsziel 1 abgedeckt ist. Der dritte Teil wird durch das Aufzeigen der Auswirkungen in der Tabelle ebenfalls erfolgreich erfüllt, wodurch das ganze Handlungsziel 1 mit diesem Artefakt abgedeckt wird.

### Erklärung Artefakt
Das Artefakt ist eine simple Analyse der OWASP Top Ten Web Application Security Risks des Jahres 2021. Es bietet Informationen zu den Top-Risiken, deren Erkennung, Gegenmassnahmen und möglichen Auswirkungen. Das Ziel ist es, Lesern ein umfassendes Verständnis für aktuelle Bedrohungen im Webanwendungssicherheitsbereich zu vermitteln.

### Beurteilung Umsetzung Artefakt im Hinblick auf das Handlungsziel
Ich würde mal so weit gehen und sagen, dass die Umsetzung des Artefakts im Hinblick auf das Handlungsziel erfolgreich ist. Es bietet eine klare und simple Zusammenfassung der aktuellen grössten Bedrohungen (OWASP Top Ten), erläutert die Erkennung durch Fakten/ Zahlen, zeigt Informationen zu Gegenmassnahmen und mögliche Auswirkungen auf. Die präsentierte Tabelle strukturiert die Informationen so, dass die Informationen schnell und einfach zu verstehen sind. Wenn man das ganze, aber noch kritischer betrachtet, könnte man sagen, dass eine detaillierte Aufführung von Gegenmassnahmen und Auswirkungen in einigen Fällen von Vorteil sein könnten. Es könnte auch nützlich sein, konkrete Beispiele für Angriffe in der echten Welt hinzuzufügen, um das Verständnis noch einfacher und nahbarer zu machen. Insgesamt jedoch erfüllt das Artefakt das Handlungsziel, bietet eine solide Grundlage für das Verständnis aktueller Bedrohungen.

## Handlungsziel 2

### Artefakt
```
// NewsController.cs

public class NewsController : ControllerBase
{
    private readonly INewsService _newsService;
    private readonly IUserService _userService;

    public NewsController(INewsService newsService, IUserService userService)
    {
        _newsService = newsService;
        _userService = userService;
    }

    [HttpGet]
    public IActionResult GetNews()
    {
        var newsList = _newsService.GetAllNews();
        return Ok(newsList);
    }

    [HttpGet("{id}")]
    public IActionResult GetNewsById(int id)
    {
        var news = _newsService.GetNewsById(id);
        return Ok(news);
    }

    [HttpPost]
    public IActionResult CreateNews([FromBody] NewsCreateDTO newsCreateDTO)
    {
        var userId = _userService.GetUserId();
        var createdNews = _newsService.CreateNews(newsCreateDTO, userId);
        return CreatedAtAction(nameof(GetNewsById), new { id = createdNews.Id }, createdNews);
    }


    [HttpGet]
    public IActionResult GetNews()
    {
        var newsList = _newsService.GetAllNews();
        var newsDTOList = newsList.Select(news => new NewsDTO
        {
            Id = news.Id,
            Header = news.Header,
            Detail = news.Detail,
            PostedDate = news.PostedDate,
            IsAdminNews = news.IsAdminNews,
            AuthorId = news.AuthorId,
            AuthorUsername = news.AuthorUsername
        }).ToList();

        return Ok(newsDTOList);
    }


    public class NewsDTO
    {
        public int Id { get; set; }
        public string Header { get; set; }
        public string Detail { get; set; }
        public DateTime PostedDate { get; set; }
        public bool IsAdminNews { get; set; }
        public int AuthorId { get; set; }
        public string AuthorUsername { get; set; }
    }
}
```

### Wie wurde das Handlungsziel mit dem Artefakt erreicht?
Dieser Code erfüllt das Handlungsziel, da durch die Überarbeitung des Codes im NewsController dessen Schwachstellen besser abgedeckt werden. Somit wurde eine Sicherheitslücke und ihre Ursache erkannt und eine Gegenmassnahme implementiert, welche die Daten besser schützt.

### Erklärung Artefakt
In diesem Fall ist das Artefakt eine überarbeitete Version des Codes im NewsController, welche die REST-Schnittstellen für die News-Operationen besser schützt. Das wurde erreicht durch das Erstellen eines DTO (Data Transfer Object), welches nur die notwendigen Daten enthält, um so die sensibleren Daten zu schützen. Die bereits vorhandenen Controller-Methoden wurden so angepasst, dass sie nur noch die Daten des DTOs verwenden, wodurch sichergestellt wird, dass nur die notwendigen Daten an den Client gesendet werden. Diese Änderungen dienen dazu, die Sicherheitslücke zu schliessen, die durch die unnötige Offenlegung von Daten in der REST-Schnittstelle verursacht wurde.

### Beurteilung Umsetzung Artefakt im Hinblick auf das Handlungsziel
Ich würde die Umsetzung des Artefakts auf Hinblick auf das Handlungsziel als erfolgreich sehen, da dieses das Handlungsziel 2 mehr oder weniger ziemlich gut abdeckt. Die Implementation des DTO erlaubt einem zu kontrollieren, welche Daten über die Rest-Schnittstelle freigegeben wird. Durch diese Massnahme wird das Prinzip der minimalen Berechtigungen eingehalten, da nur die für den Client relevanten Daten weitergegeben werden. Kurz gesagt trägt die Überarbeitung dazu bei, Sicherheitslücken zu schliessen und die Angriffsfläche der Anwendung zu reduzieren. Natürlich muss man auch sagen, dass damit auch weiterhin noch weitere Sicherheitsanstrengungen nötig sind und noch weitere Massnahmen vorgenommen werden müssen, um die Sicherheit der Anwendung zu erhöhen.

## Handlungsziel 3

### Artefakt

```
public class JwtAuthenticationService
{
   private readonly IConfiguration _configuration;
   public JwtAuthenticationService(IConfiguration configuration)
   {
       _configuration = configuration;
   }
   public string GenerateJwtToken(User user)
   {
       var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_configuration["Jwt:Key"]));
       var credentials = new SigningCredentials(key, SecurityAlgorithms.HmacSha512);
       var claims = new List<Claim>
       {
           new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString()),
           new Claim(JwtRegisteredClaimNames.NameId, user.Id.ToString()),
           new Claim(JwtRegisteredClaimNames.UniqueName, user.Username),
           new Claim(ClaimTypes.Role, user.IsAdmin ? "admin" : "user")
       };
       var tokenDescriptor = new SecurityTokenDescriptor
       {
           Subject = new ClaimsIdentity(claims),
           Expires = DateTime.UtcNow.AddHours(24),
           SigningCredentials = credentials,
           Issuer = _configuration["Jwt:Issuer"],
           Audience = _configuration["Jwt:Audience"]
       };
       var tokenHandler = new JwtSecurityTokenHandler();
       var token = tokenHandler.CreateToken(tokenDescriptor);
       return tokenHandler.WriteToken(token);
   }
}

---

[ApiController]
[Route("api/[controller]")]

public class LoginController : ControllerBase

{

    private readonly JwtAuthenticationService _jwtAuthenticationService;

    public LoginController(JwtAuthenticationService jwtAuthenticationService)

    {

        _jwtAuthenticationService = jwtAuthenticationService;

    }

    [HttpPost("authenticate")]

    public IActionResult Authenticate([FromBody] LoginRequestModel loginModel)

    {


        var user = ValidateCredentials(loginModel);

        if (user == null)

            return Unauthorized();

        var token = _jwtAuthenticationService.GenerateJwtToken(user);

        return Ok(new { Token = token });

    }

}
```

### Wie wurde das Handlungsziel mit dem Artefakt erreicht?
Das Handlungsziel wird durch das gezeigte Artefakt in sofern abgedeckt, da mit diesem ein JWT-basiertes Authentifizierungssystem in eine C#-Anwendung implementiert werden könnte. Damit wird eine sichere Generierung von Tokens für User sichergestellt, welche sich erfolgreich authentifiziert haben.

### Erklärung Artefakt

### Beurteilung Umsetzung Artefakt im Hinblick auf das Handlungsziel

## Handlungsziel 4
![Artefakt des Handlungsziel 4](-)

### Wie wurde das Handlungsziel mit dem Artefakt erreicht?

### Erklärung Artefakt

### Beurteilung Umsetzung Artefakt im Hinblick auf das Handlungsziel

## Handlungsziel 5
![Artefakt des Handlungsziel 5](-)

## Selbsteinschätzung des Erreichungsgrades der Kompetenz des Moduls

### Wie wurde das Handlungsziel mit dem Artefakt erreicht?

### Erklärung Artefakt

### Beurteilung Umsetzung Artefakt im Hinblick auf das Handlungsziel

# Lesson: Digital & Serious Games

### First and Last Name: Artemis Maria Malliakoudi
### University Registration Number: dpsd19070
### GitHub Personal Profile: https://github.com/Artem1sM
### Digital & Serious Games Personal Repository: https://github.com/Artem1sM/Role-Playing-Game

# Introduction
Δημιουργία ψηφιακού παιχνιδιού μέσα απο το πρόγραμμα unity που θα μπορεί να παιχτεί από browser και html5.   

# Summary
Πρωταγωνιστής του παιχνιδιού είναι ο snoopy ο οποίος έχει την δυνατότητα να μετακινηθεί και να περιηγηθεί μέσα στην πίστα, στην οποία έχω τοποθετήσει διάφορα αντικείμενα στον χώρο ενώ παράλληλα έχει 5 ζωές.Ωστόσο μέσα στην πίστα βρίσκεται μια περιοχή (φωτιά) όπου ο snoopy μπορεί να χάσει ζωές και ένας εχθρός (ρομπότ) που μπορεί να του τις μειώσει. Εντούτοις, μέσα στην πίστα υπάρχουν και ζωές (καρδιές) που μπορεί να κερδίσει ο χαρακτήρας μας.

# 1st Deliverable
Αρχικά δημιούργησα ένα νέο project στο Unity και ακολουθώντας τις οδηγίες του tutorial https://learn.unity.com/tutorial/main-character-and-first-script?uv=2020.3&projectId=5c6166dbedbc2a0021b1bc7c κατάφερα να φτίαξω τον κώδικα για να μετακινώ τον χαρακτήρα μου.
Πρώτα έφτιαξα τον χαρακτήρα όπως λέει στο tutorial και το script για να κινείται.Μόλις τελείωσα το πρώτο tutorial αντικατέστησα τον χαρακτήρα της ruby με μια εικόνα απο τον χαρακτήρα του snoopy που βρήκα στο internet. Για την δημιουργία του tilemap χρειάστηκε να δώ ένα extra video https://www.youtube.com/watch?v=DTp5zi8_u1U απο το youtube το οποίο είχε επισυνάψει ένας συμφοιτητής μου στα σχόλια, απο το βίντεο πήρα λοιπόν την έτοιμη εικόνα που είχε στην περιγραφή του video. Με την εικόνα αυτή έφτιαξα ένα tile palette και χρησιμοποίησα το palette  για να φτιάξω το tilemap ώστε να ολοκληρωθεί η πίστα. Στην συνέχεια, για να προσθέσω τα διάφορα αντικείμενα έκανα χρήση του ίδιου palette που χρησιμοποίησα για την κατασκευή του tilemap, δημιούργησα κάποια prefabs απο τα αντικείμενα που έχει το palette όπως λέει στο tutorial και τέλος έβαλα κάποια αντικείμενα στην πίστα μου.

# 2nd Deliverable
Για την δημιουργία εμποδίων, αρχικά πρόσθεσα ένα καινουργιο component(inspector->add component-> rigidbody 2d) στον χαρακτήρα μου, τον snoopy. Μέσα στο rigidbody 2d όρισα το gravity scale: 0. Έπειτα επέλεξα inspector menu->overrides->apply , ώστε το gravity scale να ορίζεται στο 0. Στην συνέχεια πρόσθεσα ένα καινούργιο component στο inspector του snoopy, το box collider 2d και όρισα το μέγεθος του τετραγώνου κόντα στις διαστάσεις του snoopy από το edit collider. Ωστόσο στην πορεία συνηδητοποίησα ότι ο χαρακτήρας μου μόλις ερχόταν σε επαφή με το εμπόδιο περιστρεφόταν και για να σταματήσει αυτό πήγα στο rigidbody 2d και επέλεξα το freeze rotation Z.Για να σταματήσει το τρέμουλο στον χαρακτήρα μου πήγα στο script->character controller->

public class CharacterController : MonoBehaviour
{
    Rigidbody2D rigidbody2d;
    float horizontal; 
    float vertical;
    
    // Start is called before the first frame update
    void Start()
    {
        rigidbody2d = GetComponent<Rigidbody2D>();
    }

    // Update is called once per frame
    void Update()
    {
        horizontal = Input.GetAxis("Horizontal");
        vertical = Input.GetAxis("Vertical");
    }

    void FixedUpdate()
    {
        Vector2 position = rigidbody2d.position;
        position.x = position.x + 3.0f * horizontal * Time.deltaTime;
        position.y = position.y + 3.0f * vertical * Time.deltaTime;

        rigidbody2d.MovePosition(position);
    }
} 

Στην συνέχεια πήγα στο Hierarchy->tilemap->inspector->add component->Tilemap Collider 2D και ύστερα στο project window->assets->art->tile και επέλεξα τα tile που δεν περιείχαν δέντρα, έτσι όταν πάτησα το play και οδήγησα την χαρακτήρα μου σε ένα δέντρο δεν μπορουσε να περάσει απο πάνω του σταματουσε στα όρια.
Έπειτα πρόσθεσα ένα καινούργιο component στο tilemap , το Composite Collider 2D και στα ήδη υπάρχον component, στο rigidbody 2d όρισα το body type σε static και στο Tilemap Collider 2D επέλεξα το used by composite

Για να προσθέσω ζωή στον χαρακτήρα μου ,snoopy, πήγα στο script->character controller και πρόσθεσα στο public class-> 

    public int maxHealth = 5;
    int currentHealth;
    Στο void class πρόσθεσα: currentHealth = maxHealth;
    Τέλος όρισα μια καινούργια void:
    public void ChangeHealth(int amount)
    {
        currentHealth = Mathf.Clamp(currentHealth + amount, 0, maxHealth);
        Debug.Log(currentHealth + "/" + maxHealth);
    }
    Για να προσθέσω to collectible thing έκανα import new asset->prefab mode->hierarchy. Στην συνέχεια πήγα στο project window->assets ->script->create->c# script-> HealthCollectible και έκανα drag and drop το script στο hierarchy-> heart-png-15 και στο Inspector->Box collider 2d->επέλεξα το is trigger. Στο script έβαλα τον παρακάτω κώδικα:
    public class HealthCollectible : MonoBehaviour
{
   void OnTriggerEnter2D(Collider2D other)
        {
        CharacterController controller = other.GetComponent<CharacterController>();

            if (controller != null)
            {
            if (controller.health < controller.maxHealth)
            {
                controller.ChangeHealth(1);
                Destroy(gameObject);
            }
            }
        }
}
Και πρόσθεσα στο Character controller script :public class:

public int health { get { return currentHealth; }}

Για τους εχθρούς:
Αρχικά πήγα στο script: Character controller και όρισα το currentHealth = maxHealth;
Για να προσθέσω του εχθρούς( φωτιά και ρομπότ) ακολουθησα την ίδια διαδικασία, εκανα import new asset->prefab mode->hierarchy. Στην συνέχεια για την φωτιά πήγα στο project window->assets ->script->create->c# script-> Damage zone. Έκανα drag and drop το Damage zone script->Hierarchy->Fire και στο inspector->add component ->Box collider 2d και επέλεξα το is trigger μετά έκανα add a Sprite Renderer component. Στην συνέχεια πήγα στο hierarchy επέλεξα τον χαρακτήρα μου snoopy και επέλεξα στο inspector->rigidbody 2d->Sleeping mode->never sleep. Sto scipt του damage zone έβαλα τον παρακάτω κώδικα:

public class DamageZone : MonoBehaviour
{
    void OnTriggerEnter2D(Collider2D other)
    {
        RubyController controller = other.GetComponent<RubyController >();

        if (controller != null)
        {
            controller.ChangeHealth(-1);
        }
    }
}

Έπειτα πήγα στο character controller script και πρόσθεσα στο :

public class CharacterController : MonoBehaviour 
public float timeInvincible = 2.0f
bool isInvincible;
    float invincibleTimer;
Στην void update:

if (isInvincible)
        {
            invincibleTimer -= Time.deltaTime;
            if (invincibleTimer < 0)
                isInvincible = false;
        }
Στην public void ChangeHealth(int amount):

if (amount < 0)
        {
            if (isInvincible)
                return;
            
            isInvincible = true;
            invincibleTimer = timeInvincible;
        }
Για τον εχθρό robot->project window->assets ->script->create->c# script-> Enemy controller. Έκανα drag and drop το Enemy controller script->Hierarchy->Robot και στο inspector->add component ->Box collider 2d  και έκανα άλλο ένα add component ->rigidbody 2d->gravity scale:0 .Τέλος επέλεξα->freeze rotation Z μετά έκανα add a Sprite Renderer component.

Στο Enemy controller script έβαλα τον παρακάτω κώδικα:
public class EnemyController : MonoBehaviour
{
    public float speed;
    public bool vertical;
    public float changeTime = 3.0f;

    Rigidbody2D rigidbody2D;
    float timer;
    int direction = 1;

    // Start is called before the first frame update
    void Start()
    {
        rigidbody2D = GetComponent<Rigidbody2D>();
        timer = changeTime;
    }
    void Update()
    {
        timer -= Time.deltaTime;

        if (timer < 0)
        {
            direction = -direction;
            timer = changeTime;
        }
    }
    void FixedUpdate()
    {
        Vector2 position = rigidbody2D.position;

        if (vertical)
        {
            position.y = position.y + Time.deltaTime * speed * direction; ;
        }
        else
        {
            position.x = position.x + Time.deltaTime * speed * direction; ;
        }

        rigidbody2D.MovePosition(position);
    }
    void OnCollisionEnter2D(Collision2D other)
    {
        CharacterController player = other.gameObject.GetComponent<CharacterController>();

        if (player != null)
        {
            player.ChangeHealth(-1);
        }
    }
}

Μερικές φωτογραφίες που απεικονίζουν τον χαρακτήρα μας (snoopy) να κινείται μέσα στην πίστα , να χάνει ζωές όταν ακουμπήσει στην damage zone (φωτιά), να κερδίζει ζωές όταν πάρει καρδίες και τέλος να χάσει ζωές όταν έρθει σε επαφή με τον εχθρό (ρομπότ).
![Στιγμιότυπο οθόνης (462)](https://user-images.githubusercontent.com/101417427/212424735-cb711b88-d097-485b-8963-f85d8fbc8a6b.png)
![Στιγμιότυπο οθόνης (463)](https://user-images.githubusercontent.com/101417427/212424862-a7ebee89-efb4-4f44-9a30-d19244c23eb9.png)
![Στιγμιότυπο οθόνης (464)](https://user-images.githubusercontent.com/101417427/212424934-6da614c7-c563-4363-82b8-9794c3860d1d.png)
![Στιγμιότυπο οθόνης (465)](https://user-images.githubusercontent.com/101417427/212424988-dcc0803a-0e41-47ea-b580-32fdbb19a221.png)
![Στιγμιότυπο οθόνης (466)](https://user-images.githubusercontent.com/101417427/212425034-7c1869a8-5979-4fa7-80fc-bbf1dca23323.png)

# 3rd Deliverable 


# Conclusions
Εν κατακλείδι, το παιχνίδι μου είναι ημιτελές, δυστυχώς δεν πρόλαβα να κάνω το 3 deliverable .Ωστόσο είμαι αρκετά ικανοποιημένη σύμφωνα με τα ζητούμενα που κατάφερα να φέρω εις πέρας μιας και δημιούργησα τον χαρακτήρα του παιχνιδιού μου, του πρόσθεσα την δυνατότητα την κίνησης και της αλληλεπίδρασης με damage zone και έναν εχθρό .Στην συνέχεια θα ήθελα να ολοκληρώσω όλα τα βήματα για το 3 deliverable, να προσθέσω καινούργιους εχθρούς και να επεκτείνω την πίστα μου, να υπάρχουν δίαφορα σημεία στην πίστα που το καθένα περιλαμβάνει μία απο τις 4 εποχές.
# Sources

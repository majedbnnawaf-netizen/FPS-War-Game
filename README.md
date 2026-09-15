# FPS-War-Game
لعبة إطلاق نار من منظور الشخص الأول - FPS War Game
# 📁 هيكل مشروع لعبة الحرب FPS

## 📂 هيكل الملفات

## 🎯 الفئات الرئيسية

### Player System
- **PlayerController**: التحكم بحركة اللاعب والقفز
- **CameraController**: كاميرا الرؤية والنظر
- **HealthSystem**: نظام الصحة والدروع والشفاء

### Weapon System
- **Weapon** (مجرد): الفئة الأساسية لجميع الأسلحة
- **WeaponSystem**: إدارة تبديل الأسلحة والإطلاق
- **RifleWeapon**: بندقية رشاش بانتشار عشوائي
- **ShotgunWeapon**: بندقية صيد بعدة رصاصات
- **SniperWeapon**: بندقية قناصة بتكبير

### Vehicle System
- **VehicleBase** (مجرد): الفئة الأساسية للمركبات
- **Tank**: دبابة بدرع قوي ومدفع رئيسي
- **Helicopter**: هليكوبتر بارتفاع متغير ومدافع
- **APC**: ناقلة جنود برشاش ومقاعد للركاب
- **Drone**: درون بدوران دوار وقنابل

### Enemy System
- **EnemyBase** (مجرد): العدو الأساسي بـ AI
- **Soldier**: جندي بندقية بنظام مطاردة وإطلاق

### Items System
- **Ammo**: الذخيرة للالتقاط
- **HealthKit**: مجموعة الإسعافات الأولية
- **ArmorKit**: مجموعة الدروع

## ⚙️ الميزات المضمونة

✅ نظام حركة سلس
✅ نظام كاميرا احترافي
✅ نظام أسلحة متنوع
✅ نظام مركبات كامل
✅ نظام صحة ودروع
✅ نظام أعداء بـ AI
✅ نظام جمع الأشياء
✅ واجهة مستخدم أساسية
✅ مدير اللعبة والموجات

## 🔧 الإضافات المستقبلية

- [ ] تأثيرات جزيئية
- [ ] أصوات وموسيقى
- [ ] نماذج ثلاثية الأبعاد
- [ ] رسومات وإضاءة
- [ ] مستويات متعددة
- [ ] نظام الحفظ
- [ ] قائمة الإعدادات
- [ ] وضع متعدد اللاعبين

---

**اللعبة جاهزة للتطوير!** 🎮
# 🎮 لعبة الحرب FPS - War Game

لعبة إطلاق نار من منظور الشخص الأول (FPS) مع أسلحة متنوعة ومركبات حربية.

## 📋 الميزات

### 🎯 الأساسيات
- نظام اللاعب والحركة
- نظام الكاميرا ثلاثية الأبعاد
- نظام الصحة والأضرار
- نظام الأسلحة

### 🔫 الأسلحة
- بندقية الرشاش (Assault Rifle)
- بندقية القناصة (Sniper Rifle)
- بندقية الصيد (Shotgun)
- مسدس الرشاش (SMG)
- رمية القنابل (Grenade Launcher)
- السكين (Melee)

### 🚗 المركبات والمعدات
- **دبابة (Tank)** - درع قوي وسلاح رئيسي
- **ناقلة جنود (APC)** - نقل سريع للجنود
- **جيب عسكري (Jeep)** - سرعة عالية
- **طائرة هليكوبتر (Helicopter)** - قنابل وأسلحة جوية
- **درون (Drone)** - استطلاع وقصف

### 🛡️ نظام الحماية
- الدروع الشخصية
- الدروع على المركبات
- الملاجئ

### 📦 أشياء احتياطية
- ذخيرة إضافية
- مجموعات إسعافات أولية
- معززات الصحة

## 🗂️ هيكل المشروع

## 🚀 البدء

1. استخدم Unity 2022 أو أحدث
2. استنسخ المستودع
3. افتح المشروع في Unity
4. شغل المشهد الرئيسية

---
**تطوير لعبة حرب رائعة! 🎯**
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    [SerializeField] private float moveSpeed = 5f;
    [SerializeField] private float sprintSpeed = 10f;
    [SerializeField] private float jumpForce = 5f;
    [SerializeField] private float groundDrag = 5f;
    [SerializeField] private float airDrag = 2f;
    [SerializeField] private float groundDist = 0.4f;
    
    private Rigidbody rb;
    private Transform groundCheck;
    private bool isGrounded;
    private Vector3 moveDirection;
    private float currentSpeed;

    private void Start()
    {
        rb = GetComponent<Rigidbody>();
        groundCheck = transform.Find("GroundCheck");
        if (groundCheck == null)
        {
            GameObject check = new GameObject("GroundCheck");
            check.transform.parent = transform;
            check.transform.localPosition = new Vector3(0, -0.5f, 0);
            groundCheck = check.transform;
        }
    }

    private void Update()
    {
        HandleInput();
        HandleGroundCheck();
        SpeedControl();
    }

    private void FixedUpdate()
    {
        Move();
    }

    private void HandleInput()
    {
        float horizontal = Input.GetAxis("Horizontal");
        float vertical = Input.GetAxis("Vertical");

        moveDirection = transform.forward * vertical + transform.right * horizontal;

        if (Input.GetKeyDown(KeyCode.Space) && isGrounded)
        {
            Jump();
        }

        currentSpeed = Input.GetKey(KeyCode.LeftShift) ? sprintSpeed : moveSpeed;
    }

    private void HandleGroundCheck()
    {
        isGrounded = Physics.CheckSphere(groundCheck.position, groundDist);
    }

    private void Move()
    {
        rb.AddForce(moveDirection.normalized * currentSpeed * 10f, ForceMode.Force);
        rb.drag = isGrounded ? groundDrag : airDrag;
    }

    private void Jump()
    {
        rb.velocity = new Vector3(rb.velocity.x, 0f, rb.velocity.z);
        rb.AddForce(transform.up * jumpForce, ForceMode.Impulse);
    }

    private void SpeedControl()
    {
        Vector3 flatVel = new Vector3(rb.velocity.x, 0f, rb.velocity.z);

        if (flatVel.magnitude > currentSpeed)
        {
            Vector3 limitedVel = flatVel.normalized * currentSpeed;
            rb.velocity = new Vector3(limitedVel.x, rb.velocity.y, limitedVel.z);
        }
    }
}
using UnityEngine;

public class CameraController : MonoBehaviour
{
    [SerializeField] private float mouseSensitivity = 2f;
    [SerializeField] private float maxUpAngle = 90f;
    [SerializeField] private float maxDownAngle = -90f;
    
    private float xRotation = 0f;
    private Transform playerBody;

    private void Start()
    {
        playerBody = transform.parent;
        Cursor.lockState = CursorLockMode.Locked;
    }

    private void Update()
    {
        HandleMouseLook();
    }

    private void HandleMouseLook()
    {
        float mouseX = Input.GetAxis("Mouse X") * mouseSensitivity;
        float mouseY = Input.GetAxis("Mouse Y") * mouseSensitivity;

        // دوران أفقي للجسم
        playerBody.Rotate(Vector3.up * mouseX);

        // دوران عمودي للكاميرا
        xRotation -= mouseY;
        xRotation = Mathf.Clamp(xRotation, maxDownAngle, maxUpAngle);

        transform.localRotation = Quaternion.Euler(xRotation, 0f, 0f);
    }

    private void OnDestroy()
    {
        Cursor.lockState = CursorLockMode.None;
    }
}
using UnityEngine;
using UnityEngine.Events;

public class HealthSystem : MonoBehaviour
{
    [SerializeField] private float maxHealth = 100f;
    [SerializeField] private float maxArmor = 50f;
    
    private float currentHealth;
    private float currentArmor;
    
    public UnityEvent<float> onHealthChanged = new UnityEvent<float>();
    public UnityEvent<float> onArmorChanged = new UnityEvent<float>();
    public UnityEvent onDeath = new UnityEvent();

    private void Start()
    {
        currentHealth = maxHealth;
        currentArmor = maxArmor;
    }

    public void TakeDamage(float damage)
    {
        float armorAbsorb = damage * 0.5f;
        
        if (currentArmor > 0)
        {
            float armorDamage = Mathf.Min(armorAbsorb, currentArmor);
            currentArmor -= armorDamage;
            damage -= armorDamage;
        }

        currentHealth -= damage;
        onHealthChanged?.Invoke(currentHealth);

        if (currentHealth <= 0)
        {
            Die();
        }
    }

    public void Heal(float amount)
    {
        currentHealth = Mathf.Min(currentHealth + amount, maxHealth);
        onHealthChanged?.Invoke(currentHealth);
    }

    public void AddArmor(float amount)
    {
        currentArmor = Mathf.Min(currentArmor + amount, maxArmor);
        onArmorChanged?.Invoke(currentArmor);
    }

    private void Die()
    {
        onDeath?.Invoke();
        Destroy(gameObject);
    }

    public float GetHealth() => currentHealth;
    public float GetArmor() => currentArmor;
    public float GetHealthPercent() => currentHealth / maxHealth;
    public float GetArmorPercent() => currentArmor / maxArmor;
}
using UnityEngine;

public class WeaponSystem : MonoBehaviour
{
    [SerializeField] private Weapon[] weapons;
    private int currentWeaponIndex = 0;
    private Weapon currentWeapon;

    private void Start()
    {
        if (weapons.Length > 0)
        {
            currentWeapon = weapons[0];
            currentWeapon.gameObject.SetActive(true);
        }
    }

    private void Update()
    {
        HandleWeaponSwitch();
        HandleShooting();
    }

    private void HandleWeaponSwitch()
    {
        // تبديل الأسلحة من 1-6
        for (int i = 0; i < weapons.Length; i++)
        {
            if (Input.GetKeyDown(KeyCode.Alpha1 + i))
            {
                SwitchWeapon(i);
            }
        }

        // تبديل بعجلة الماوس
        if (Input.GetKeyDown(KeyCode.E))
        {
            SwitchWeapon((currentWeaponIndex + 1) % weapons.Length);
        }
    }

    private void SwitchWeapon(int index)
    {
        if (index == currentWeaponIndex) return;

        currentWeapon.gameObject.SetActive(false);
        currentWeaponIndex = index;
        currentWeapon = weapons[index];
        currentWeapon.gameObject.SetActive(true);
        currentWeapon.OnEquip();
    }

    private void HandleShooting()
    {
        if (Input.GetMouseButton(0))
        {
            currentWeapon.Fire();
        }

        if (Input.GetMouseButtonDown(1))
        {
            currentWeapon.AimDownSights();
        }

        if (Input.GetMouseButtonUp(1))
        {
            currentWeapon.StopAiming();
        }

        if (Input.GetKeyDown(KeyCode.R))
        {
            currentWeapon.Reload();
        }
    }

    public Weapon GetCurrentWeapon() => currentWeapon;
}
using UnityEngine;

public abstract class Weapon : MonoBehaviour
{
    [SerializeField] protected string weaponName;
    [SerializeField] protected float damage = 25f;
    [SerializeField] protected float fireRate = 0.1f;
    [SerializeField] protected int magazineSize = 30;
    [SerializeField] protected float reloadTime = 2f;
    [SerializeField] protected float range = 100f;
    [SerializeField] protected Transform muzzlePoint;
    
    protected int currentAmmo;
    protected int totalAmmo;
    protected float lastFireTime = 0f;
    protected bool isReloading = false;
    protected bool isAiming = false;

    protected virtual void Start()
    {
        currentAmmo = magazineSize;
        totalAmmo = magazineSize * 3;
    }

    public virtual void Fire()
    {
        if (isReloading || currentAmmo <= 0) return;
        if (Time.time - lastFireTime < fireRate) return;

        lastFireTime = Time.time;
        currentAmmo--;
        
        OnFire();
    }

    protected abstract void OnFire();

    public virtual void Reload()
    {
        if (isReloading || currentAmmo == magazineSize) return;
        
        StartCoroutine(ReloadCoroutine());
    }

    protected System.Collections.IEnumerator ReloadCoroutine()
    {
        isReloading = true;
        yield return new WaitForSeconds(reloadTime);
        
        int ammoNeeded = magazineSize - currentAmmo;
        int ammoToAdd = Mathf.Min(ammoNeeded, totalAmmo);
        
        currentAmmo += ammoToAdd;
        totalAmmo -= ammoToAdd;
        isReloading = false;
    }

    public virtual void AimDownSights()
    {
        isAiming = true;
    }

    public virtual void StopAiming()
    {
        isAiming = false;
    }

    public virtual void OnEquip() { }

    public int GetCurrentAmmo() => currentAmmo;
    public int GetTotalAmmo() => totalAmmo;
    public string GetWeaponName() => weaponName;
    public bool IsReloading() => isReloading;
}
using UnityEngine;

public class RifleWeapon : Weapon
{
    [SerializeField] private float bulletSpread = 2f;
    [SerializeField] private LayerMask hitLayer;

    protected override void OnFire()
    {
        Vector3 shootDirection = muzzlePoint.forward;
        
        // إضافة انتشار عشوائي
        float randomSpread = isAiming ? bulletSpread * 0.5f : bulletSpread;
        shootDirection += new Vector3(
            Random.Range(-randomSpread, randomSpread),
            Random.Range(-randomSpread, randomSpread),
            Random.Range(-randomSpread, randomSpread)
        );

        Ray ray = new Ray(muzzlePoint.position, shootDirection);
        
        if (Physics.Raycast(ray, out RaycastHit hit, range, hitLayer))
        {
            // إلحاق الضرر بالهدف
            HealthSystem healthSystem = hit.collider.GetComponent<HealthSystem>();
            if (healthSystem != null)
            {
                healthSystem.TakeDamage(damage);
            }
using UnityEngine;

public class ShotgunWeapon : Weapon
{
    [SerializeField] private int pelletsPerShot = 8;
    [SerializeField] private float spreadAngle = 15f;
    [SerializeField] private LayerMask hitLayer;

    private void Start()
    {
        base.Start();
        fireRate = 0.5f; // إطلاق أبطأ
        damage = 60f; // ضرر أعلى
    }

    protected override void OnFire()
    {
        for (int i = 0; i < pelletsPerShot; i++)
        {
            FirePellet();
        }
    }

    private void FirePellet()
    {
        Vector3 shootDirection = muzzlePoint.forward;
        
        // انتشار عشوائي أكبر
        float randomX = Random.Range(-spreadAngle, spreadAngle);
        float randomY = Random.Range(-spreadAngle, spreadAngle);
        
        shootDirection = Quaternion.Euler(randomX, randomY, 0) * shootDirection;

        Ray ray = new Ray(muzzlePoint.position, shootDirection);
        
        if (Physics.Raycast(ray, out RaycastHit hit, range, hitLayer))
        {
            HealthSystem healthSystem = hit.collider.GetComponent<HealthSystem>();
            if (healthSystem != null)
            {
                healthSystem.TakeDamage(damage / pelletsPerShot);
            }
        }
    }
}
using UnityEngine;

public class SniperWeapon : Weapon
{
    [SerializeField] private float zoomAmount = 5f;
    [SerializeField] private LayerMask hitLayer;
    private Camera mainCamera;
    private float originalFOV;

    private void Start()
    {
        base.Start();
        fireRate = 1.5f; // إطلاق بطيء
        damage = 120f; // ضرر عالي جداً
        mainCamera = Camera.main;
        originalFOV = mainCamera.fieldOfView;
    }

    public override void AimDownSights()
    {
        base.AimDownSights();
        mainCamera.fieldOfView = originalFOV / zoomAmount;
    }

    public override void StopAiming()
    {
        base.StopAiming();
        mainCamera.fieldOfView = originalFOV;
    }

    protected override void OnFire()
    {
        Ray ray = new Ray(muzzlePoint.position, muzzlePoint.forward);
        
        if (Physics.Raycast(ray, out RaycastHit hit, range, hitLayer))
        {
            HealthSystem healthSystem = hit.collider.GetComponent<HealthSystem>();
            if (healthSystem != null)
            {
                healthSystem.TakeDamage(damage);
                Debug.Log("إصابة مباشرة!");
            }
        }
    }
}
using UnityEngine;

public class CameraController : MonoBehaviour
{
    [SerializeField] private float mouseSensitivity = 2f;
    [SerializeField] private float maxUpAngle = 90f;
    [SerializeField] private float maxDownAngle = -90f;
    
    private float xRotation = 0f;
    private Transform playerBody;

    private void Start()
    {
        playerBody = transform.parent;
        Cursor.lockState = CursorLockMode.Locked;
    }

    private void Update()
    {
        HandleMouseLook();
    }

    private void HandleMouseLook()
    {
        float mouseX = Input.GetAxis("Mouse X") * mouseSensitivity;
        float mouseY = Input.GetAxis("Mouse Y") * mouseSensitivity;

        // دوران أفقي للجسم
        playerBody.Rotate(Vector3.up * mouseX);

        // دوران عمودي للكاميرا
        xRotation -= mouseY;
        xRotation = Mathf.Clamp(xRotation, maxDownAngle, maxUpAngle);

        transform.localRotation = Quaternion.Euler(xRotation, 0f, 0f);
    }

    private void OnDestroy()
    {
        Cursor.lockState = CursorLockMode.None;
    }
}
using UnityEngine;
using UnityEngine.Events;

public class HealthSystem : MonoBehaviour
{
    [SerializeField] private float maxHealth = 100f;
    [SerializeField] private float maxArmor = 50f;
    
    private float currentHealth;
    private float currentArmor;
    
    public UnityEvent<float> onHealthChanged = new UnityEvent<float>();
    public UnityEvent<float> onArmorChanged = new UnityEvent<float>();
    public UnityEvent onDeath = new UnityEvent();

    private void Start()
    {
        currentHealth = maxHealth;
        currentArmor = maxArmor;
    }

    public void TakeDamage(float damage)
    {
        float armorAbsorb = damage * 0.5f;
        
        if (currentArmor > 0)
        {
            float armorDamage = Mathf.Min(armorAbsorb, currentArmor);
            currentArmor -= armorDamage;
            damage -= armorDamage;
        }

        currentHealth -= damage;
        onHealthChanged?.Invoke(currentHealth);

        if (currentHealth <= 0)
        {
            Die();
        }
    }

    public void Heal(float amount)
    {
        currentHealth = Mathf.Min(currentHealth + amount, maxHealth);
        onHealthChanged?.Invoke(currentHealth);
    }

    public void AddArmor(float amount)
    {
        currentArmor = Mathf.Min(currentArmor + amount, maxArmor);
        onArmorChanged?.Invoke(currentArmor);
    }

    private void Die()
    {
        onDeath?.Invoke();
        Destroy(gameObject);
    }

    public float GetHealth() => currentHealth;
    public float GetArmor() => currentArmor;
    public float GetHealthPercent() => currentHealth / maxHealth;
    public float GetArmorPercent() => currentArmor / maxArmor;
}
using UnityEngine;

public class WeaponSystem : MonoBehaviour
{
    [SerializeField] private Weapon[] weapons;
    private int currentWeaponIndex = 0;
    private Weapon currentWeapon;

    private void Start()
    {
        if (weapons.Length > 0)
        {
            currentWeapon = weapons[0];
            currentWeapon.gameObject.SetActive(true);
        }
    }

    private void Update()
    {
        HandleWeaponSwitch();
        HandleShooting();
    }

    private void HandleWeaponSwitch()
    {
        // تبديل الأسلحة من 1-6
        for (int i = 0; i < weapons.Length; i++)
        {
            if (Input.GetKeyDown(KeyCode.Alpha1 + i))
            {
                SwitchWeapon(i);
            }
        }

        // تبديل بعجلة الماوس
        if (Input.GetKeyDown(KeyCode.E))
        {
            SwitchWeapon((currentWeaponIndex + 1) % weapons.Length);
        }
    }

    private void SwitchWeapon(int index)
    {
        if (index == currentWeaponIndex) return;

        currentWeapon.gameObject.SetActive(false);
        currentWeaponIndex = index;
        currentWeapon = weapons[index];
        currentWeapon.gameObject.SetActive(true);
        currentWeapon.OnEquip();
    }

    private void HandleShooting()
    {
        if (Input.GetMouseButton(0))
        {
            currentWeapon.Fire();
        }

        if (Input.GetMouseButtonDown(1))
        {
            currentWeapon.AimDownSights();
        }

        if (Input.GetMouseButtonUp(1))
        {
            currentWeapon.StopAiming();
        }

        if (Input.GetKeyDown(KeyCode.R))
        {
            currentWeapon.Reload();
        }
    }

    public Weapon GetCurrentWeapon() => currentWeapon;
}
using UnityEngine;

public abstract class Weapon : MonoBehaviour
{
    [SerializeField] protected string weaponName;
    [SerializeField] protected float damage = 25f;
    [SerializeField] protected float fireRate = 0.1f;
    [SerializeField] protected int magazineSize = 30;
    [SerializeField] protected float reloadTime = 2f;
    [SerializeField] protected float range = 100f;
    [SerializeField] protected Transform muzzlePoint;
    
    protected int currentAmmo;
    protected int totalAmmo;
    protected float lastFireTime = 0f;
    protected bool isReloading = false;
    protected bool isAiming = false;

    protected virtual void Start()
    {
        currentAmmo = magazineSize;
        totalAmmo = magazineSize * 3;
    }

    public virtual void Fire()
    {
        if (isReloading || currentAmmo <= 0) return;
        if (Time.time - lastFireTime < fireRate) return;

        lastFireTime = Time.time;
        currentAmmo--;
        
        OnFire();
    }

    protected abstract void OnFire();

    public virtual void Reload()
    {
        if (isReloading || currentAmmo == magazineSize) return;
        
        StartCoroutine(ReloadCoroutine());
    }

    protected System.Collections.IEnumerator ReloadCoroutine()
    {
        isReloading = true;
        yield return new WaitForSeconds(reloadTime);
        
        int ammoNeeded = magazineSize - currentAmmo;
        int ammoToAdd = Mathf.Min(ammoNeeded, totalAmmo);
        
        currentAmmo += ammoToAdd;
        totalAmmo -= ammoToAdd;
        isReloading = false;
    }

    public virtual void AimDownSights()
    {
        isAiming = true;
    }

    public virtual void StopAiming()
    {
        isAiming = false;
    }

    public virtual void OnEquip() { }

    public int GetCurrentAmmo() => currentAmmo;
    public int GetTotalAmmo() => totalAmmo;
    public string GetWeaponName() => weaponName;
    public bool IsReloading() => isReloading;
}
using UnityEngine;

public class RifleWeapon : Weapon
{
    [SerializeField] private float bulletSpread = 2f;
    [SerializeField] private LayerMask hitLayer;

    protected override void OnFire()
    {
        Vector3 shootDirection = muzzlePoint.forward;
        
        // إضافة انتشار عشوائي
        float randomSpread = isAiming ? bulletSpread * 0.5f : bulletSpread;
        shootDirection += new Vector3(
            Random.Range(-randomSpread, randomSpread),
            Random.Range(-randomSpread, randomSpread),
            Random.Range(-randomSpread, randomSpread)
        );

        Ray ray = new Ray(muzzlePoint.position, shootDirection);
        
        if (Physics.Raycast(ray, out RaycastHit hit, range, hitLayer))
        {
            // إلحاق الضرر بالهدف
            HealthSystem healthSystem = hit.collider.GetComponent<HealthSystem>();
            if (healthSystem != null)
            {
                healthSystem.TakeDamage(damage);
            }

            // تأثير بصري عند الإصابة
            CreateHitEffect(hit.point, hit.normal);
        }

        // تأثير بصري للطلقة
        CreateMuzzleFlash();
    }

    private void CreateMuzzleFlash()
    {
        // يمكن إضافة تأثيرات جزيئية هنا
        Debug.Log($"{weaponName} أطلقت طلقة!");
    }

    private void CreateHitEffect(Vector3 position, Vector3 normal)
    {
        // يمكن إضافة تأثيرات الاصطدام هنا
        Debug.Log($"ضربة في {position}");
    }
}
using UnityEngine;

public class ShotgunWeapon : Weapon
{
    [SerializeField] private int pelletsPerShot = 8;
    [SerializeField] private float spreadAngle = 15f;
    [SerializeField] private LayerMask hitLayer;

    private void Start()
    {
        base.Start();
        fireRate = 0.5f; // إطلاق أبطأ
        damage = 60f; // ضرر أعلى
    }

    protected override void OnFire()
    {
        for (int i = 0; i < pelletsPerShot; i++)
        {
            FirePellet();
        }
    }

    private void FirePellet()
    {
        Vector3 shootDirection = muzzlePoint.forward;
        
        // انتشار عشوائي أكبر
        float randomX = Random.Range(-spreadAngle, spreadAngle);
        float randomY = Random.Range(-spreadAngle, spreadAngle);
        
        shootDirection = Quaternion.Euler(randomX, randomY, 0) * shootDirection;

        Ray ray = new Ray(muzzlePoint.position, shootDirection);
        
        if (Physics.Raycast(ray, out RaycastHit hit, range, hitLayer))
        {
            HealthSystem healthSystem = hit.collider.GetComponent<HealthSystem>();
            if (healthSystem != null)
            {
                healthSystem.TakeDamage(damage / pelletsPerShot);
            }
        }
    }
}
using UnityEngine;

public class SniperWeapon : Weapon
{
    [SerializeField] private float zoomAmount = 5f;
    [SerializeField] private LayerMask hitLayer;
    private Camera mainCamera;
    private float originalFOV;

    private void Start()
    {
        base.Start();
        fireRate = 1.5f; // إطلاق بطيء
        damage = 120f; // ضرر عالي جداً
        mainCamera = Camera.main;
        originalFOV = mainCamera.fieldOfView;
    }

    public override void AimDownSights()
    {
        base.AimDownSights();
        mainCamera.fieldOfView = originalFOV / zoomAmount;
    }

    public override void StopAiming()
    {
        base.StopAiming();
        mainCamera.fieldOfView = originalFOV;
    }

    protected override void OnFire()
    {
        Ray ray = new Ray(muzzlePoint.position, muzzlePoint.forward);
        
        if (Physics.Raycast(ray, out RaycastHit hit, range, hitLayer))
        {
            HealthSystem healthSystem = hit.collider.GetComponent<HealthSystem>();
            if (healthSystem != null)
            {
                healthSystem.TakeDamage(damage);
                Debug.Log("إصابة مباشرة!");
            }
        }
    }
}
using UnityEngine;

public abstract class VehicleBase : MonoBehaviour
{
    [SerializeField] protected string vehicleName;
    [SerializeField] protected float maxHealth = 200f;
    [SerializeField] protected float maxSpeed = 30f;
    [SerializeField] protected float acceleration = 10f;
    [SerializeField] protected float rotationSpeed = 5f;
    [SerializeField] protected int maxPassengers = 4;
    
    protected float currentHealth;
    protected float currentSpeed = 0f;
    protected Rigidbody rb;
    protected int passengersCount = 0;

    protected virtual void Start()
    {
        currentHealth = maxHealth;
        rb = GetComponent<Rigidbody>();
    }

    public virtual void Accelerate(float input)
    {
        currentSpeed = Mathf.Lerp(currentSpeed, maxSpeed * input, Time.deltaTime * acceleration);
        rb.velocity = transform.forward * currentSpeed;
    }

    public virtual void Rotate(float input)
    {
        transform.Rotate(Vector3.up * input * rotationSpeed);
    }

    public virtual void TakeDamage(float damage)
    {
        currentHealth -= damage;
        
        if (currentHealth <= 0)
        {
            Destroy(gameObject);
        }
    }

    public virtual bool AddPassenger()
    {
        if (passengersCount < maxPassengers)
        {
            passengersCount++;
            return true;
        }
        return false;
    }

    public virtual void RemovePassenger()
    {
        if (passengersCount > 0)
            passengersCount--;
    }

    public float GetHealth() => currentHealth;
    public float GetHealthPercent() => currentHealth / maxHealth;
    public int GetPassengersCount() => passengersCount;
}
using UnityEngine;

public class Tank : VehicleBase
{
    [SerializeField] private Transform turret;
    [SerializeField] private Transform barrel;
    [SerializeField] private Transform shellSpawnPoint;
    [SerializeField] private float shellForce = 100f;
    [SerializeField] private float fireRate = 3f;
    
    private float lastFireTime = 0f;

    protected override void Start()
    {
        base.Start();
        maxHealth = 500f;
        maxSpeed = 15f;
        vehicleName = "دبابة";
    }

    public void RotateTurret(float input)
    {
        if (turret != null)
        {
            turret.Rotate(Vector3.up * input * rotationSpeed);
        }
    }

    public void RotateBarrel(float input)
    {
        if (barrel != null)
        {
            barrel.Rotate(Vector3.right * input * rotationSpeed);
        }
    }

    public void FireMainGun()
    {
        if (Time.time - lastFireTime < fireRate) return;
        
        lastFireTime = Time.time;
        
        // إنشاء قذيفة
        // يمكن إضافة prefab هنا
        Debug.Log("إطلاق نار من الدبابة!");
    }

    public override void TakeDamage(float damage)
    {
        // الدبابة لديها درع أقوى
        base.TakeDamage(damage * 0.7f);
    }
}
using UnityEngine;

public class Helicopter : VehicleBase
{
    [SerializeField] private float altitude = 20f;
    [SerializeField] private float verticalSpeed = 5f;
    [SerializeField] private Transform[] gunPositions;
    [SerializeField] private float gunFireRate = 0.1f;
    
    private float currentAltitude;
    private float[] lastGunFireTime;

    protected override void Start()
    {
        base.Start();
        maxHealth = 300f;
        maxSpeed = 40f;
        vehicleName = "هليكوبتر";
        currentAltitude = transform.position.y;
        lastGunFireTime = new float[gunPositions.Length];
    }

    public void ChangeAltitude(float input)
    {
        currentAltitude += input * verticalSpeed * Time.deltaTime;
        currentAltitude = Mathf.Clamp(currentAltitude, 10f, 100f);
        
        Vector3 pos = transform.position;
        pos.y = currentAltitude;
        transform.position = pos;
    }

    public void FireGuns()
    {
        for (int i = 0; i < gunPositions.Length; i++)
        {
            if (Time.time - lastGunFireTime[i] >= gunFireRate)
            {
                lastGunFireTime[i] = Time.time;
                FireGun(i);
            }
        }
    }

    private void FireGun(int gunIndex)
    {
        Debug.Log($"إطلاق نار من المدفع {gunIndex}!");
    }

    public void FireMissiles()
    {
        Debug.Log("إطلاق الصواريخ!");
    }
}
using UnityEngine;

public class APC : VehicleBase
{
    [SerializeField] private Transform machineGun;
    [SerializeField] private float gunRotationSpeed = 10f;
    [SerializeField] private float fireRate = 0.15f;
    
    private float lastFireTime = 0f;

    protected override void Start()
    {
        base.Start();
        maxHealth = 250f;
        maxSpeed = 35f;
        maxPassengers = 8;
        vehicleName = "ناقلة جنود";
    }

    public void RotateMachineGun(float horizontalInput, float verticalInput)
    {
        if (machineGun != null)
        {
            machineGun.Rotate(Vector3.up * horizontalInput * gunRotationSpeed);
            machineGun.Rotate(Vector3.right * verticalInput * gunRotationSpeed);
        }
    }

    public void FireMachineGun()
    {
        if (Time.time - lastFireTime < fireRate) return;
        
        lastFireTime = Time.time;
        Debug.Log("إطلاق نار من رشاش ناقلة الجنود!");
    }

    public override bool AddPassenger()
    {
        if (base.AddPassenger())
        {
            Debug.Log($"جندي دخل الآلية. العدد الحالي: {passengersCount}");
            return true;
        }
        return false;
    }
}
using UnityEngine;

public class Drone : VehicleBase
{
    [SerializeField] private float rotorSpeed = 200f;
    [SerializeField] private Transform[] rotors;
    [SerializeField] private float bombForce = 50f;
    [SerializeField] private float bombDamage = 100f;
    
    private bool isEquippedWithBombs = true;

    protected override void Start()
    {
        base.Start();
        maxHealth = 100f;
        maxSpeed = 50f;
        vehicleName = "درون";
    }

    private void Update()
    {
        RotateRotors();
    }

    private void RotateRotors()
    {
        foreach (Transform rotor in rotors)
        {
            if (rotor != null)
                rotor.Rotate(Vector3.up * rotorSpeed);
        }
    }

    public void DropBomb()
    {
        if (!isEquippedWithBombs)
        {
            Debug.Log("لا توجد قنابل!");
            return;
        }

        isEquippedWithBombs = false;
        Debug.Log("تم إسقاط القنبلة!");
    }

    public void Rearm()
    {
        isEquippedWithBombs = true;
        Debug.Log("تم تسليح الدرون!");
    }

    public void TakeAerialPhoto()
    {
        Debug.Log("التقاط صورة جوية!");
    }
}
using UnityEngine;

public class Ammo : MonoBehaviour
{
    [SerializeField] private int ammoAmount = 30;
    [SerializeField] private string weaponType = "Rifle";
    
    private void OnTriggerEnter(Collider collision)
    {
        if (collision.CompareTag("Player"))
        {
            WeaponSystem weaponSystem = collision.GetComponent<WeaponSystem>();
            if (weaponSystem != null)
            {
                // إضافة الذخيرة
                Debug.Log($"التقطت {ammoAmount} ذخيرة {weaponType}!");
                Destroy(gameObject);
            }
        }
    }
}
using UnityEngine;

public class HealthKit : MonoBehaviour
{
    [SerializeField] private float healAmount = 50f;
    [SerializeField] private bool isHighTier = false;
    
    private void Start()
    {
        if (isHighTier)
            healAmount = 100f;
    }
    
    private void OnTriggerEnter(Collider collision)
    {
        if (collision.CompareTag("Player"))
        {
            HealthSystem healthSystem = collision.GetComponent<HealthSystem>();
            if (healthSystem != null)
            {
                healthSystem.Heal(healAmount);
                Debug.Log($"تم الشفاء بـ {healAmount} صحة!");
                Destroy(gameObject);
            }
        }
    }
}
using UnityEngine;

public class ArmorKit : MonoBehaviour
{
    [SerializeField] private float armorAmount = 30f;
    
    private void OnTriggerEnter(Collider collision)
    {
        if (collision.CompareTag("Player"))
        {
            HealthSystem healthSystem = collision.GetComponent<HealthSystem>();
            if (healthSystem != null)
            {
                healthSystem.AddArmor(armorAmount);
                Debug.Log($"تم إضافة {armorAmount} درع!");
                Destroy(gameObject);
            }
        }
    }
}
using UnityEngine;
using UnityEngine.AI;

public class EnemyBase : MonoBehaviour
{
    [SerializeField] protected string enemyName;
    [SerializeField] protected float maxHealth = 100f;
    [SerializeField] protected float attackDamage = 15f;
    [SerializeField] protected float attackRange = 2f;
    [SerializeField] protected float sightRange = 30f;
    [SerializeField] protected float attackCooldown = 1f;
    
    protected float currentHealth;
    protected NavMeshAgent navAgent;
    protected Transform playerTransform;
    protected float lastAttackTime = 0f;
    protected bool isAlerted = false;

    protected virtual void Start()
    {
        currentHealth = maxHealth;
        navAgent = GetComponent<NavMeshAgent>();
        playerTransform = GameObject.FindGameObjectWithTag("Player")?.transform;
    }

    protected virtual void Update()
    {
        if (playerTransform == null) return;

        float distanceToPlayer = Vector3.Distance(transform.position, playerTransform.position);

        if (distanceToPlayer <= sightRange)
        {
            isAlerted = true;
            Chase();

            if (distanceToPlayer <= attackRange)
            {
                Attack();
            }
        }
        else
        {
            isAlerted = false;
            Patrol();
        }
    }

    protected virtual void Chase()
    {
        navAgent.SetDestination(playerTransform.position);
    }

    protected virtual void Patrol()
    {
        // يمكن إضافة نقاط دورية هنا
    }

    protected virtual void Attack()
    {
        if (Time.time - lastAttackTime < attackCooldown) return;

        lastAttackTime = Time.time;
        HealthSystem playerHealth = playerTransform.GetComponent<HealthSystem>();
        if (playerHealth != null)
        {
            playerHealth.TakeDamage(attackDamage);
        }
    }

    public virtual void TakeDamage(float damage)
    {
        currentHealth -= damage;
        
        if (currentHealth <= 0)
        {
            Die();
        }
    }

    protected virtual void Die()
    {
        Destroy(gameObject);
    }

    public float GetHealth() => currentHealth;
}
using UnityEngine;

public class Soldier : EnemyBase
{
    [SerializeField] private float fireRate = 1f;
    [SerializeField] private float shootRange = 20f;
    [SerializeField] private Transform gunMuzzle;
    
    private float lastShootTime = 0f;

    protected override void Start()
    {
        base.Start();
        enemyName = "جندي";
        maxHealth = 50f;
    }

    protected override void Chase()
    {
        base.Chase();
        
        float distanceToPlayer = Vector3.Distance(transform.position, playerTransform.position);
        
        if (distanceToPlayer <= shootRange && CanSeePlayer())
        {
            Shoot();
        }
    }

    private void Shoot()
    {
        if (Time.time - lastShootTime < fireRate) return;
        
        lastShootTime = Time.time;
        
        Ray ray = new Ray(gunMuzzle.position, gunMuzzle.forward);
        if (Physics.Raycast(ray, out RaycastHit hit, shootRange))
        {
            HealthSystem health = hit.collider.GetComponent<HealthSystem>();
            if (health != null)
            {
                health.TakeDamage(10f);
            }
        }
    }

    private bool CanSeePlayer()
    {
        Vector3 directionToPlayer = (playerTransform.position - transform.position).normalized;
        float angleToPlayer = Vector3.Angle(transform.forward, directionToPlayer);
        
        return angleToPlayer < 45f;
    }
}
using UnityEngine;
using UnityEngine.UI;
using TMPro;

public class HUD : MonoBehaviour
{
    [SerializeField] private Image healthBar;
    [SerializeField] private Image armorBar;
    [SerializeField] private TextMeshProUGUI ammoText;
    [SerializeField] private TextMeshProUGUI weaponNameText;
    [SerializeField] private TextMeshProUGUI objectiveText;
    
    private HealthSystem playerHealth;
    private WeaponSystem weaponSystem;

    private void Start()
    {
        playerHealth = FindObjectOfType<HealthSystem>();
        weaponSystem = FindObjectOfType<WeaponSystem>();
        
        if (playerHealth != null)
            playerHealth.onHealthChanged.AddListener(UpdateHealthBar);
    }

    private void Update()
    {
        UpdateAmmoDisplay();
        UpdateWeaponDisplay();
    }

    private void UpdateHealthBar(float health)
    {
        if (healthBar != null)
        {
            healthBar.fillAmount = playerHealth.GetHealthPercent();
        }
        
        if (armorBar != null)
        {
            armorBar.fillAmount = playerHealth.GetArmorPercent();
        }
    }

    private void UpdateAmmoDisplay()
    {
        if (weaponSystem != null && ammoText != null)
        {
            Weapon weapon = weaponSystem.GetCurrentWeapon();
            if (weapon != null)
            {
                ammoText.text = $"{weapon.GetCurrentAmmo()} / {weapon.GetTotalAmmo()}";
            }
        }
    }

    private void UpdateWeaponDisplay()
    {
        if (weaponSystem != null && weaponNameText != null)
        {
            Weapon weapon = weaponSystem.GetCurrentWeapon();
            if (weapon != null)
            {
                weaponNameText.text = weapon.GetWeaponName();
            }
        }
    }

    public void SetObjective(string text)
    {
        if (objectiveText != null)
            objectiveText.text = text;
    }
}
using UnityEngine;

public class GameManager : MonoBehaviour
{
    public static GameManager Instance { get; private set; }
    
    [SerializeField] private int initialEnemyCount = 10;
    [SerializeField] private float waveSpawnInterval = 30f;
    
    private int currentWave = 1;
    private int enemiesDefeated = 0;
    private int totalScore = 0;
    private bool isGameOver = false;

    private void Awake()
    {
        if (Instance == null)
        {
            Instance = this;
            DontDestroyOnLoad(gameObject);
        }
        else
        {
            Destroy(gameObject);
        }
    }

    private void Start()
    {
        SpawnWave(currentWave);
        InvokeRepeating(nameof(SpawnNextWave), waveSpawnInterval, waveSpawnInterval);
    }

    private void SpawnWave(int waveNumber)
    {
        int enemiesToSpawn = initialEnemyCount + (waveNumber - 1) * 3;
        Debug.Log($"بدء الموجة {waveNumber}! عدد الأعداء: {enemiesToSpawn}");
        
        // يمكن إضافة منطق الإظهار هنا
    }

    private void SpawnNextWave()
    {
        currentWave++;
        SpawnWave(currentWave);
    }

    public void AddScore(int points)
    {
        totalScore += points;
    }

    public void EnemyDefeated()
    {
        enemiesDefeated++;
        AddScore(100);
    }

    public void GameOver()
    {
        isGameOver = true;
        Time.timeScale = 0f;
        Debug.Log($"انتهت اللعبة! النقاط الكلية: {totalScore}");
    }

    public int GetScore() => totalScore;
    public int GetWave() => currentWave;
    public bool IsGameOver() => isGameOver;
}
# Logs
*.log
*.logs

# Unity specific
/[Ll]ibrary/
/[Tt]emp/
/[Oo]bj/
/[Bb]uild/
/[Bb]uilds/
/[Ll]ogs/

# Visual Studio cache/options directory
.vs/
.vscode/

# Visual Studio Code specific
.vscode/launch.json

# Rider IDE
.idea/
*.sln.iml

# macOS
.DS_Store
*.swp
*.swo

# Python
__pycache__/
*.py[cod]
*$py.class

# Node
node_modules/
package-lock.json

# Project specific
Assets/Plugins/
Assets/AssetStore/
# Logs
*.log
*.logs

# Unity specific
/[Ll]ibrary/
/[Tt]emp/
/[Oo]bj/
/[Bb]uild/
/[Bb]uilds/
/[Ll]ogs/

# Visual Studio cache/options directory
.vs/
.vscode/

# Visual Studio Code specific
.vscode/launch.json

# Rider IDE
.idea/
*.sln.iml

# macOS
.DS_Store
*.swp
*.swo

# Python
__pycache__/
*.py[cod]
*$py.class

# Node
node_modules/
package-lock.json

# Project specific
Assets/Plugins/
Assets/AssetStore/
# Logs
*.log
*.logs

# Unity specific
/[Ll]ibrary/
/[Tt]emp/
/[Oo]bj/
/[Bb]uild/
/[Bb]uilds/
/[Ll]ogs/

# Visual Studio cache/options directory
.vs/
.vscode/

# Visual Studio Code specific
.vscode/launch.json

# Rider IDE
.idea/
*.sln.iml

# macOS
.DS_Store
*.swp
*.swo

# Python
__pycache__/
*.py[cod]
*$py.class

# Node
node_modules/
package-lock.json

# Project specific
Assets/Plugins/
Assets/AssetStore/

            // تأثير بصري عند الإصابة
            CreateHitEffect(hit.point, hit.normal);
        }

        // تأثير بصري للطلقة
        CreateMuzzleFlash();
    }

    private void CreateMuzzleFlash()
    {
        // يمكن إضافة تأثيرات جزيئية هنا
        Debug.Log($"{weaponName} أطلقت طلقة!");
    }

    private void CreateHitEffect(Vector3 position, Vector3 normal)
    {
        // يمكن إضافة تأثيرات الاصطدام هنا
        Debug.Log($"ضربة في {position}");
    }
}

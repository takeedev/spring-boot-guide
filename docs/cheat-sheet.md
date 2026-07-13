# Cheat Sheet — สรุป Annotation ทั้งหมด


```
เริ่มแอป          → @SpringBootApplication
รับ Request       → @RestController, @GetMapping, @PostMapping
ดึงข้อมูลจาก URL   → @PathVariable, @RequestParam, @RequestBody
Business Logic    → @Service
คุย Database      → @Repository, @Entity, @Id
Inject Bean       → constructor injection (หรือ @Autowired)
Bean ชนิดซ้ำกัน    → @Qualifier("ชื่อ") ที่จุดฉีด, @Primary ที่ตัว default
วงจรชีวิต Bean     → @PostConstruct, @PreDestroy, @Scope
Transaction       → @Transactional (readOnly, rollbackFor, propagation)
งานตามเวลา/เบื้องหลัง → @Scheduled, @Async (+ @EnableScheduling, @EnableAsync)
Cache             → @Cacheable, @CacheEvict (+ @EnableCaching)
Event             → @EventListener, ApplicationEventPublisher
Lombok            → @Getter, @Builder, @RequiredArgsConstructor, @Slf4j
ตรวจสอบข้อมูล     → @Valid, @NotBlank, @Email
จัดการ Error      → @RestControllerAdvice, @ExceptionHandler
อ่าน Config       → @Value, @ConfigurationProperties
ความสัมพันธ์ตาราง  → @OneToMany, @ManyToOne, @JoinColumn
เขียน Test        → @Test, @BeforeEach, @Mock/@InjectMocks (JUnit/Mockito)
                    @SpringBootTest, @WebMvcTest, @DataJpaTest, @MockitoBean (Spring)
                    @ActiveProfiles, @WithMockUser (สภาพแวดล้อม/Security)
Security          → @EnableWebSecurity, @PreAuthorize
```


---

⬅️ [บทที่ 12: Annotation เพิ่มพลัง](12-power-annotations.md) | [🏠 สารบัญ](../README.md)

# Spring Boot

## Spring Boot Test

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = {
        XdpStorageServiceImpl.class,
        WasbTestCaseProvider.class,
        XdpTestCaseProvider.class,
})
@TestPropertySource(
        properties = {
        "spring.cloud.config.enabled = false"
})
public class XdpStorageServiceImplTest {

    @MockBean
    CIPServiceClient cipServiceClient;

    @MockBean
    XdpSasTokenRepository xdpSasTokenRepository;

    @MockBean
    RestTemplate restTemplate;


    @Before
    public void setUp() {
        Mockito.when(cipServiceClient.loggedInTenantDetails()).thenReturn(tenantVo);
        Mockito.when(restTemplate.exchange(ArgumentMatchers.anyString(), ArgumentMatchers.eq(HttpMethod.POST), ArgumentMatchers.<HttpEntity>any(), ArgumentMatchers.eq(Void.class)))
                .thenReturn(responseEntity);
    }

    @Test
    public void checkStorageEnabledAndGetTenantDetails(){
        Assert.assertTrue(xdpStorageService.isStorageEnabled());
    }
    

}
```

## PowerMock

```java
@RunWith(PowerMockRunner.class)
@PrepareForTest({AuthUtil.class, AesServiceImpl.class})
public class AesServiceImplTest {

    @Mock
    private CipService cipService;

    @Mock
    private AppProps appProps;

    @InjectMocks
    private AesServiceImpl aesService;



    @Before
    public void setUp() throws Exception {
        PowerMockito.mockStatic(AuthUtil.class);
        PowerMockito.when(AuthUtil.getUserDetails()).thenReturn(userVo);
    }
```


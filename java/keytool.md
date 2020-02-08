# Keytool

## Generage  jks

```bash
keytool -genkey -alias jwt -keyalg RSA -keysize 1024 -keystore jwt.jks -validity 365
```

## Get private & public key

```java
import java.io.IOException;
import java.io.InputStream;
import java.security.KeyStore;
import java.security.KeyStoreException;
import java.security.NoSuchAlgorithmException;
import java.security.PrivateKey;
import java.security.PublicKey;
import java.security.UnrecoverableKeyException;
import java.security.cert.CertificateException;

import lombok.extern.slf4j.Slf4j;

/**
 * @author jiangyuanlin@163.com
 *
 */

@Slf4j
public class JksUtil {
	public static void main(String[] args) throws Exception {
		PrivateKey privateKey = getPrivateKey("jwt.jks", "123456", "jwt");
		log.info("" + privateKey);
		PublicKey publicKey = getPublicKey("jwt.jks", "123456", "jwt");
		log.info("" + publicKey);

	}

	private static PrivateKey getPrivateKey(String fileName, String password, String alias) throws KeyStoreException,
			IOException, NoSuchAlgorithmException, CertificateException, UnrecoverableKeyException {
		InputStream inputStream = Thread.currentThread().getContextClassLoader().getResourceAsStream(fileName);

		KeyStore keyStore = KeyStore.getInstance("JKS");
		keyStore.load(inputStream, "123456".toCharArray());

		return (PrivateKey) keyStore.getKey("jwt", "123456".toCharArray());

	}

	private static PublicKey getPublicKey(String fileName, String password, String alias) throws KeyStoreException,
			IOException, NoSuchAlgorithmException, CertificateException, UnrecoverableKeyException {
		InputStream inputStream = Thread.currentThread().getContextClassLoader().getResourceAsStream(fileName);

		KeyStore keyStore = KeyStore.getInstance("JKS");
		keyStore.load(inputStream, "123456".toCharArray());

		return keyStore.getCertificate("jwt").getPublicKey();

	}

}
```


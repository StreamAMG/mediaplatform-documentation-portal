The following helper function can be used in Java to generate a KSession Token. This Token can then be used to access MediaPlatform Content.

Simply copy the following class into Java Project. 

```java
import java.nio.charset.StandardCharsets;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Base64;
import java.util.Date;

public class KSessionHelper {

    public static int ADMIN_TYPE = 2;
    public static int USER_TYPE = 0;

    private static final String privilegesPattern = "sview:%s,setrole:PLAYBACK_BASE_ROLE,widget:1";
    private static final String infoPattern = "%s;%s;%d;%s;%d;%s;%s;";

    /**
     *
     * This helper function generates a kSession token to be used to access StreamAMG Content.
     *
     * @param entryId The Entry ID for which you wish to generate a KSession Token
     * @param partnerId The Partner ID of the MediaPlatform Instance
     * @param secret The Admin or User Secret to be used to mint the KSession Token
     * @param type The Type of Token you are creating either Admin or User
     * @param user The Username to Audit the KSession creation, optional.
     * @param expiry The Expiry in Seconds for the Token
     *
     * @return Generated KSession Token
     */
    public static String generateKSession(String entryId, String partnerId, String secret, int type, String user, int expiry ) throws NoSuchAlgorithmException {

        // Get the current time so we can calculate the expiry time
        long now = new Date().getTime()/1000;

        String privileges = String.format(privilegesPattern, entryId);
        String info = String.format(infoPattern, partnerId, partnerId, now + expiry, type, now, user, privileges);

        MessageDigest md = MessageDigest.getInstance("SHA-1");
        md.update((secret + info).getBytes(StandardCharsets.UTF_8));
        String signature = byteArrayToHexString(md.digest());

        return Base64.getEncoder().encodeToString((signature + "|" + info).getBytes());
    }

    /**
     * Hash Function
     *
     * @param b Byte Array to Convert to Hex String
     * @return Hex Hash Value
     */
    public static String byteArrayToHexString(byte[] b) {
        StringBuilder result = new StringBuilder();
        for (byte value : b) {
            result.append(Integer.toString((value & 0xff) + 0x100, 16).substring(1));
        }
        return result.toString();
    }
}

```
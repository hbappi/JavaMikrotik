# JavaMikrotik

# Summary
RouterOS API access library written in Java. This code is also capable to connect to IPv6 addresses.

# Licensing
Code is provided as is and can be freely used freely. I, as a writer of code, am not responsible for anything that may arise from use of this code.

# Usage
Simple example how this can be used. T3apiView class is the interface class that is not supplied here and is mentioned here only do show, how to start simple listener thread to receive the data replied by RouterOS
```java
       ApiConnection ret = new ApiConnection("192.168.88.1", 8728);
       if (!ret.isConnected()) {
           ret.start();
           try {
               ret.join();
               if (ret.isConnected()) {
                   ret.login("admin", new char[0]);
               }
           } catch (InterruptedException ex) {
               Logger.getLogger(T3apiView.class.getName()).log(Level.SEVERE, null, ex);
               return null;
           }
       }
       aConn.sendCommand("/ip/address/print");
       DataReceiver dataRec = new DataReceiver(aConn, this);
       dataRec.start();
```
# DataReceiver Class
```java
      import libAPI.*;
      import java.util.logging.Level;
      import java.util.logging.Logger;

      /**
      *
      * @author janisk
      */
      public class DataReceiver extends Thread {

       private ApiConnection aConn = null;
       T3apiView t3A = null;

       public DataReceiver(ApiConnection aConn, T3apiView t3A) {
           this.aConn = aConn;
           this.t3A = t3A;
       }

       @Override
       public void run() {
           String s = "";
           while (true) {
               try {
                   s = aConn.getData();
                   if (s != null) {
                       t3A.outputHere(s);
                       if (s.contains("!done")) {
                       }
                   }
               } catch (InterruptedException ex) {
                   Logger.getLogger(DataReceiver.class.getName()).log(Level.SEVERE, null, ex);
               }
           }
       }
      } 
```
      
# Code
Code that is ready to be compiled and used. In some places some comments may be missing. [Compiled jar file](http://www.mikrotik.com/download/libAPI.jar) of same java classes

# See also
[API](https://wiki.mikrotik.com/wiki/API) 
<br>
[Usage notes on API](https://wiki.mikrotik.com/wiki/API_command_notes)

### really-executable-jars-maven-plugin
---
https://github.com/brianm/really-executable-jars-maven-plugin

```java
// src/main/java/org/skife/waffles/ReallyExecutableJarMojo.java

public class ReallyExecutableJarMojo extends AbstractMojo
{
  
  private void makeExecutable(File file)
    throws IOException,MojoExecutionException
  {
    getLog().debug();
    
    Path original = Paths.get();
    Files.move();
    try (final FileOutputStream out = new FileOutputStream(file);
        final InputStream in = Files.newInputStream(original)) {
      
      if (scriptFile == null) {
        out.write("#!/bin/sh\n\nexec java " + flags + " -jar \"$0\" \"$@\"\n\n").getBytes("ASCII"));
      }
      else if (Files.exists(Paths.get(scriptFile))) {
        getLog().debug(String.format("Loading file[%s] from filesystem", scriptFile));
        
        byte[] script = Files.readAllBytes(Paths.get(scriptFile));
        out.write(script);
        out.write(new byte[]{'\n', '\n'});
      }
      else {
        getLog().debug(String.format("Loading file[%s] from jar{%s]", scriptFile, original));
        
        try (final URLClassLoader loader = new URLClassLoader(new URL[]{original.toUri().toURL()}, null);
          final InputStream scriptIn = loader.getResourceAsStream(scriptFile)) {
          
          out.write(IOUtil.toString(scriptIn).getBytes("ASCII"));
          out.write("\n\n".getBytes("ASCII"));
        }
      }
      IOUtil.copy(in, out);
    }
    finally {
      Files.deletefExists(original);
    }
    
    file.setExecutable(true, false);
    
    getLog().info(String.format("Successfully made JAR [%s] executable", file.getAbsolutePath()));
  }
}
```

```
```

```
```



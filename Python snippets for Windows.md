#Python snippets for Windows
This can be used for automating similar workflows in windows using Python
Source -[_ipython notebook_](http://nbviewer.jupyter.org/github/winpython/winpython_afterdoc/blob/master/examples/using_cython_and_numba_under_winpython.ipynb) [_archive.is_](http://archive.is/zWhA5)

## Setting up environment variables. 

```py
os.environ["tmp_Rdirectory"]= "R" 
os.environ["R_HOME"] = os.environ["WINPYDIR"]+ "\\..\\tools\\R\\" 
os.environ["R_HOMEbin"]=os.environ["R_HOME"] + "bin" 
# for installation we need this
os.environ["tmp_Rbase"]=os.path.join(os.path.split(os.environ["WINPYDIR"])[0]  , 'tools','R' )
```

## Using urllib2 to download the file. 

```py
try:
    import urllib.request as urllib2  # Python 3
except:
    import urllib2  # Python 2

# specify R binary and hash
r_url = "https://cran.r-project.org/bin/windows/base/R-3.2.3-win.exe"
hashes=("5572ce407831ce9156485d4f896ad616", "b86077268e4aa5498482f4514202009009ddf947" )

r_url = "https://cran.r-project.org/bin/windows/base/R-3.2.3-win.exe"

# specify target location
r_binary = os.path.basename(r_url)
r_installer = os.environ["WINPYDIR"]+"\\..\\tools\\"+r_binary

# Download
g = urllib2.urlopen(r_url) 
with io.open(r_installer, 'wb') as f:
    f.write(g.read())
g.close
g = None    
```

## Using environment variables

```py
os.environ["r_installer"] = r_installer

#checking it's there
!dir %r_installer%
```

## How to check hash value of a file

```py
import hashlib
def give_hash(of_file, with_this):
    with io.open(r_installer, 'rb') as f:
        return with_this(f.read()).hexdigest()  
print (" "*12+"MD5"+" "*(32-12-3)+" "+" "*15+"SHA-1"+" "*(40-15-5)+"\n"+"-"*32+" "+"-"*40)

print ("%s %s %s" % (give_hash(r_installer, hashlib.md5) , give_hash(r_installer, hashlib.sha1),r_installer))
assert give_hash(r_installer, hashlib.md5) == hashes[0]
assert give_hash(r_installer, hashlib.sha1) == hashes[1]
```

```
            MD5                                 SHA-1                    
-------------------------------- ----------------------------------------
5572ce407831ce9156485d4f896ad616 b86077268e4aa5498482f4514202009009ddf947 D:\result_tests\winpython-3.5.1.amd64\python-3.5.1.amd64\..\tools\R-3.2.3-win.exe
```

## Pass compiler options based on 32bit or 64 bit

```py
if 'amd64' in sys.version.lower():
    r_comp ='/COMPONENTS="main,x64,translations'
else:
    r_comp ='/COMPONENTS="main,i386,translations'

# for installation we need this
os.environ["tmp_Rbase"]=os.path.join(os.path.split(os.environ["WINPYDIR"])[0]  , 'tools','R' )


os.environ["tmp_R_comp"]=r_comp
!start cmd /C %r_installer% /DIR=%tmp_Rbase% %tmp_R_comp%
```




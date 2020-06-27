# Ejercicios de Android Studio

### Ejercicio 7
problema:
Disponer un ListView con bs nombres de paises de sudamérica. Cuando se seleccione un país mostrar en un TextView
("Large Text") la cantidad de habitantes del país seleccionado.
La interfaz visual a implementar es siguiente (primero disponemos un TextView (*Large Text") (llamado tvl) y un
ListView (llamado listView por defecto))

### Resolusion
En la interfaz visual implementamos lo siguiente 
- (primero disponemos un TextView en la parte superior (cuyo id lo definimos con el valor tv1) y un ListView (y definimos su id con el valor list1))

![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/7_1.png)

### Codigo:

```sh
package com.example.ejercicio_7;


import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ListView;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {

    private String[] paises = {"Argentina", "Chile", "Paraguay", "Bolivia",
            "Peru", "Ecuador", "Brasil", "Colombia", "Venezuela", "Uruguay"};
    private String[] habitantes = {"40000000", "17000000", "6500000",
            "10000000", "30000000", "14000000", "183000000", "44000000",
            "29000000", "3500000"};
    private TextView tvl;
    private ListView lvl;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        tvl = (TextView) findViewById(R.id.tv1);
        lvl = (ListView) findViewById(R.id.listView);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this, android.R.layout
                .simple_list_item_1, paises);
        lvl.setAdapter(adapter);
        lvl.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView parent, View view, int position, long id) {
                tvl.setText("Población de " + lvl.getItemAtPosition(position) + " es " + habitantes[position]);
            }
        });
    }
    
        }
```
Resultado:

![resultado](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/ejercicio_7.png?raw=true)


# 12 - Lanzar un segundo "Activity" y pasar parámetros
problema:
Confeccionar un programa que solicite el ingrese de una dirección de un sitio web y seguidamente abrir una segunda ventana que muestre dicha página.
1 - Nuestro primer Activity tendrá la siguiente interfaz visual (ver controles):


Para resolver este problema utilizaremos el control visual WebView que nos permite mostrar el contenido de un sitio web.
![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/12-1.png)

### Codigo MainActivity:
```sh
package com.example.ejercicio_12;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.EditText;

public class MainActivity extends AppCompatActivity {
    private EditText et1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        et1=(EditText)findViewById(R.id.et1);
    }
    public void ver (View v) {
        Intent i=new Intent(this,Actividad2.class);
        i.putExtra("direccion", et1.getText().toString());
        startActivity(i);
    }
}
```
2 La segunda interfaz visual (recordemos que debemos presionar el botón derecho en la ventana Project (sobre app) y seleccionar la opción New -> Activity -> Emplty Activity):

![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/12-2.png)

### Codigo Actividad2:
```
package com.example.ejercicio_12;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.webkit.WebView;

public class Actividad2 extends AppCompatActivity {
    WebView web1;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_actividad2);

        web1=(WebView)findViewById(R.id.web1);

        Bundle bundle = getIntent().getExtras();
        String dato=bundle.getString("direccion");
        web1.loadUrl("http://" + dato);
    }

    public void finalizar(View v) {
        finish();
    }
}
```
## Importante
Como nuestra aplicación debe acceder a internet debemos hacer una configuración en el archivo "AndroidManifest.xml", podemos ubicar este archivo:
![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/12-2.png)

resultado:

![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/ejercicio12.png)

![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/ejercicio12_2.png)

interfaz 2:

![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/ejercicio12_3.png)


# 28 - Grabación de audio mediante el grabador provisto por Android (via Intent)
Problema:
Disponer dos objetos de la clase Button con las etiquetas "grabar" y "reproducir". Cuando se presione el primer botón proceder a activar la grabadora provista por Android. Cuando se presione el segundo botón reproducir el audio grabado.

![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/12-2.png)

### Codigo:
```
package com.example.ejercicio_28;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.media.MediaPlayer;
import android.net.Uri;
import android.os.Bundle;
import android.provider.MediaStore;
import android.view.View;


public class MainActivity extends AppCompatActivity {
    int peticion = 1;
    Uri url1;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
    public void grabar(View v) {
        Intent intent = new Intent(MediaStore.Audio.Media.RECORD_SOUND_ACTION);
        startActivityForResult(intent, peticion);
    }

    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (resultCode == RESULT_OK && requestCode == peticion) {
            url1 = data.getData();
        }
    }
    public void reproducir(View v) {
        MediaPlayer mediaPlayer = MediaPlayer.create(this, url1);
        mediaPlayer.start();
    }


}
```
resultado:

![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/ejercicio28.png)

# 40 - Evento touch: juego del buscaminas
Problema:
Implementar el juego del Buscaminas. Crear una grilla de 8*8 celdas.

1 - Creamos un proyecto llamado: BuscaMinas
Borramos el TextView que agrega automáticamente el Android Studio y disponemos un Button y un LinearLayout:

Al botón inicializamos la propiedad onClick con el valor "reiniciar" y al LinearLayout le asignamos el ID como "layout1".
![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/40-1.png)

2 Luego codificamos las clases MainActivity. 
### Codigo:
```sh
package com.example.ejercicio_40;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.graphics.Typeface;
import android.os.Bundle;
import android.view.MotionEvent;
import android.view.View;
import android.view.Window;
import android.view.WindowManager;
import android.widget.LinearLayout;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity implements View.OnTouchListener{

    private Tablero fondo;
    int x, y;
    private Casilla[][] casillas;
    private boolean activo = true;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        requestWindowFeature(Window.FEATURE_NO_TITLE);
        getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,
                WindowManager.LayoutParams.FLAG_FULLSCREEN);
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        LinearLayout layout = (LinearLayout) findViewById(R.id.layout1);
        fondo = new Tablero(this);
        fondo.setOnTouchListener(this);
        layout.addView(fondo);
        casillas = new Casilla[8][8];
        for (int f = 0; f < 8; f++) {
            for (int c = 0; c < 8; c++) {
                casillas[f][c] = new Casilla();
            }
        }
        this.disponerBombas();
        this.contarBombasPerimetro();
        getSupportActionBar().hide();
    }

    public void reiniciar(View v) {
        casillas = new Casilla[8][8];
        for (int f = 0; f < 8; f++) {
            for (int c = 0; c < 8; c++) {
                casillas[f][c] = new Casilla();
            }
        }
        this.disponerBombas();
        this.contarBombasPerimetro();
        activo = true;

        fondo.invalidate();
    }

    @Override
    public boolean onTouch(View v, MotionEvent event) {
        if (activo)
            for (int f = 0; f < 8; f++) {
                for (int c = 0; c < 8; c++) {
                    if (casillas[f][c].dentro((int) event.getX(),
                            (int) event.getY())) {
                        casillas[f][c].destapado = true;
                        if (casillas[f][c].contenido == 80) {
                            Toast.makeText(this, "Perdiste REINICIA",
                                    Toast.LENGTH_LONG).show();
                            activo = false;
                        } else if (casillas[f][c].contenido == 0)
                            recorrer(f, c);
                        fondo.invalidate();
                    }
                }
            }
        if (gano() && activo) {
            Toast.makeText(this, "Ganaste", Toast.LENGTH_LONG).show();
            activo = false;
        }

        return true;
    }

    class Tablero extends View {

        public Tablero(Context context) {
            super(context);
        }

        protected void onDraw(Canvas canvas) {
            canvas.drawRGB(0, 0, 0);
            int ancho = 0;
            if (canvas.getWidth() < canvas.getHeight())
                ancho = fondo.getWidth();
            else
                ancho = fondo.getHeight();
            int anchocua = ancho / 8;
            Paint paint = new Paint();
            paint.setTextSize(50);
            Paint paint2 = new Paint();
            paint2.setTextSize(50);
            paint2.setTypeface(Typeface.DEFAULT_BOLD);
            paint2.setARGB(255, 0, 0, 255);
            Paint paintlinea1 = new Paint();
            paintlinea1.setARGB(255, 255, 255, 255);
            int filaact = 0;
            for (int f = 0; f < 8; f++) {
                for (int c = 0; c < 8; c++) {
                    casillas[f][c].fijarxy(c * anchocua, filaact, anchocua);
                    if (casillas[f][c].destapado == false)
                        paint.setARGB(153, 204, 204, 204);
                    else
                        paint.setARGB(255, 153, 153, 153);
                    canvas.drawRect(c * anchocua, filaact, c * anchocua
                            + anchocua - 2, filaact + anchocua - 2, paint);
                    // linea blanca
                    canvas.drawLine(c * anchocua, filaact, c * anchocua
                            + anchocua, filaact, paintlinea1);
                    canvas.drawLine(c * anchocua + anchocua - 1, filaact, c
                                    * anchocua + anchocua - 1, filaact + anchocua,
                            paintlinea1);

                    if (casillas[f][c].contenido >= 1
                            && casillas[f][c].contenido <= 8
                            && casillas[f][c].destapado)
                        canvas.drawText(
                                String.valueOf(casillas[f][c].contenido), c
                                        * anchocua + (anchocua / 2) - 8,
                                filaact + anchocua / 2, paint2);

                    if (casillas[f][c].contenido == 80
                            && casillas[f][c].destapado) {
                        Paint bomba = new Paint();
                        bomba.setARGB(255, 255, 0, 0);
                        canvas.drawCircle(c * anchocua + (anchocua / 2),
                                filaact + (anchocua / 2), 8, bomba);
                    }

                }
                filaact = filaact + anchocua;
            }
        }
    }

    private void disponerBombas() {
        int cantidad = 8;
        do {
            int fila = (int) (Math.random() * 8);
            int columna = (int) (Math.random() * 8);
            if (casillas[fila][columna].contenido == 0) {
                casillas[fila][columna].contenido = 80;
                cantidad--;
            }
        } while (cantidad != 0);
    }

    private boolean gano() {
        int cant = 0;
        for (int f = 0; f < 8; f++)
            for (int c = 0; c < 8; c++)
                if (casillas[f][c].destapado)
                    cant++;
        if (cant == 56)
            return true;
        else
            return false;
    }

    private void contarBombasPerimetro() {
        for (int f = 0; f < 8; f++) {
            for (int c = 0; c < 8; c++) {
                if (casillas[f][c].contenido == 0) {
                    int cant = contarCoordenada(f, c);
                    casillas[f][c].contenido = cant;
                }
            }
        }
    }

    int contarCoordenada(int fila, int columna) {
        int total = 0;
        if (fila - 1 >= 0 && columna - 1 >= 0) {
            if (casillas[fila - 1][columna - 1].contenido == 80)
                total++;
        }
        if (fila - 1 >= 0) {
            if (casillas[fila - 1][columna].contenido == 80)
                total++;
        }
        if (fila - 1 >= 0 && columna + 1 < 8) {
            if (casillas[fila - 1][columna + 1].contenido == 80)
                total++;
        }

        if (columna + 1 < 8) {
            if (casillas[fila][columna + 1].contenido == 80)
                total++;
        }
        if (fila + 1 < 8 && columna + 1 < 8) {
            if (casillas[fila + 1][columna + 1].contenido == 80)
                total++;
        }

        if (fila + 1 < 8) {
            if (casillas[fila + 1][columna].contenido == 80)
                total++;
        }
        if (fila + 1 < 8 && columna - 1 >= 0) {
            if (casillas[fila + 1][columna - 1].contenido == 80)
                total++;
        }
        if (columna - 1 >= 0) {
            if (casillas[fila][columna - 1].contenido == 80)
                total++;
        }
        return total;
    }

    private void recorrer(int fil, int col) {
        if (fil >= 0 && fil < 8 && col >= 0 && col < 8) {
            if (casillas[fil][col].contenido == 0) {
                casillas[fil][col].destapado = true;
                casillas[fil][col].contenido = 50;
                recorrer(fil, col + 1);
                recorrer(fil, col - 1);
                recorrer(fil + 1, col);
                recorrer(fil - 1, col);
                recorrer(fil - 1, col - 1);
                recorrer(fil - 1, col + 1);
                recorrer(fil + 1, col + 1);
                recorrer(fil + 1, col - 1);
            } else if (casillas[fil][col].contenido >= 1
                    && casillas[fil][col].contenido <= 8) {
                casillas[fil][col].destapado = true;
            }
        }
    }
}
```
3 Creamos una clase llamada Casilla desde el Android Studio:
```
package com.example.ejercicio_40;

public class Casilla {
    public int x,y,ancho;
    public int contenido=0;
    public boolean destapado=false;
    public void fijarxy(int x,int y, int ancho) {
        this.x=x;
        this.y=y;
        this.ancho=ancho;
    }

    public boolean dentro(int xx,int yy) {
        if (xx>=this.x && xx<=this.x+ancho && yy>=this.y && yy<=this.y+ancho)
            return true;
        else
            return false;
    }
}
```
resultado:

![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/ejercicio40-1.png)

![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/ejercicio40-2.png)


# 43 - Localización y archivo strings.xml
Problema:
Crear un nuevo proyecto Proyecto046 basado en el ejercicio_42 de tal manera que muestre su interfaz no solo para el idioma por defecto (español) e ingles, sino para el portugués de Brasil y el portugués de Portugal.

Las abreviaturas para los distintos lenguajes los podemos ver en la página ISO 639-1 Code (segunda columna) y para obtener las distintas regiones utilizamos la tabla ISO 3166-1-alpha-2

Para agregar un nuevo idioma de una región procedemos de la misma manera que para cuando seleccionamos un lenguaje: botón derecho sobre la carpeta values :New Values Resource File y en este diálogo especificamos que crearemos un archivo llamado strings.xml y seleccionaremos "Locate":

![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/43-1.png)
### Codigo ejercicio_41:
```
package com.example.ejercicio_43;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.EditText;
import android.widget.RadioButton;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {
    EditText et1, et2;
    RadioButton rb1, rb2;
    TextView tv1, tv2;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        et1 = (EditText) findViewById(R.id.editText);
        et2 = (EditText) findViewById(R.id.editText2);
        rb1 = (RadioButton) findViewById(R.id.radioButton);
        rb2 = (RadioButton) findViewById(R.id.radioButton2);
        tv1 = (TextView) findViewById(R.id.textView);
        tv2 = (TextView) findViewById(R.id.textView2);
    }

    public void operar(View v) {
        int v1 = Integer.parseInt(et1.getText().toString());
        int v2 = Integer.parseInt(et2.getText().toString());
        if (rb1.isChecked()) {
            int suma = v1 + v2;
            tv2.setText(String.valueOf("resultado: "+suma));
        } else if (rb2.isChecked()) {
            int resta = v1 - v2;
            tv2.setText(String.valueOf("resultado: "+resta));
        }
    }
}
```
Resultado 1:

![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/ejercicio43-1.png)


### Codigo ejercicio_42 strings_ingles:
```
<?xml version="1.0" encoding="utf-8"?>
<resources>

    <string name="app_name">Proyecto045</string>
    <string name="radiosuma">add</string>
    <string name="radioresta">subtract</string>
    <string name="botonoperacion">resolve</string>

</resources>
```

Resultado 2:

![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/ejercicio43-2.png)

### Codigo ejercicio_43 otro idioma:
```
<?xml version="1.0" encoding="utf-8"?>
<resources>

    <string name="app_name">Progetto 43</string>
    <string name="radiosuma">addizionare</string>
    <string name="radioresta">sottrarre</string>
    <string name="botonoperacion">risolvere  </string>
</resources>
```
Resultado 3

![](https://raw.githubusercontent.com/SANMH/Ejercicios_Android/master/assets/ejercicio43-3.png)


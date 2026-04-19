package com.neuromantico

import android.os.Bundle
import android.widget.*
import androidx.appcompat.app.AppCompatActivity
import java.io.File
import java.text.SimpleDateFormat
import java.util.*

class MainActivity : AppCompatActivity() {

    lateinit var editText: EditText
    lateinit var spinner: Spinner
    lateinit var button: Button
    lateinit var logView: TextView

    val fileName = "registro.json"

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        setContentView(R.layout.activity_main)

        editText = findViewById(R.id.inputText)
        spinner = findViewById(R.id.spinner)
        button = findViewById(R.id.saveButton)
        logView = findViewById(R.id.logView)

        val opciones = arrayOf("Mañana", "Tarde", "Noche")
        spinner.adapter = ArrayAdapter(this, android.R.layout.simple_spinner_dropdown_item, opciones)

        button.setOnClickListener {
            guardarRegistro()
        }

        cargarHistorial()
    }

    private fun guardarRegistro() {
        val texto = editText.text.toString()
        val momento = spinner.selectedItem.toString()

        val sdf = SimpleDateFormat("yyyy-MM-dd HH:mm:ss", Locale.getDefault())
        val fecha = sdf.format(Date())

        val json = """{"fecha":"$fecha","momento":"$momento","texto":"$texto"}\n"""

        val file = File(filesDir, fileName)
        file.appendText(json)

        editText.setText("")
        cargarHistorial()
    }

    private fun cargarHistorial() {
        val file = File(filesDir, fileName)
        if (file.exists()) {
            logView.text = file.readText()
        }
    }
}
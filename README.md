# ImageProcessingCoroutineApp

```
package com.devtides.imageprocessingcoroutines

import android.graphics.Bitmap
import android.graphics.BitmapFactory
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.View
import kotlinx.android.synthetic.main.activity_main.*
import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.async
import kotlinx.coroutines.launch
import java.net.URL

class MainActivity : AppCompatActivity() {

    private val IMAGE_URL = "https://raw.githubusercontent.com/DevTides/JetpackDogsApp/master/app/src/main/res/drawable/dog.png"

    private val coroutineScope = CoroutineScope(Dispatchers.Main)


    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        coroutineScope.launch {
            val originalDeferred = coroutineScope.async(Dispatchers.IO) {
                getOriginalBitmap()
            }
            val originalBitmap = originalDeferred.await()

            val filterDeferred = coroutineScope.async(Dispatchers.Default) { applyFilter(originalBitmap) }
            val filterBitmap = filterDeferred.await()

            loadImage(filterBitmap)
        }
    }

    private fun getOriginalBitmap() = URL(IMAGE_URL).openStream().use {
            BitmapFactory.decodeStream(it)
    }

    private fun applyFilter(originalBitmap: Bitmap) = Filter.apply(originalBitmap)

    private fun loadImage(bmp: Bitmap) {
        progressBar.visibility = View.GONE
        imageView.setImageBitmap(bmp)
        imageView.visibility = View.VISIBLE
    }
}

```
<img width="376" alt="Screen Shot 2565-04-21 at 17 05 55" src="https://user-images.githubusercontent.com/62892558/164432010-023babd1-3814-4502-965f-57782f3c9486.png">

package com.sparsh.ca1cse225

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.runtime.*
import androidx.compose.material3.*
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import kotlinx.coroutines.delay

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            CA1CSE225App()
        }
    }
}

@Composable
fun CA1CSE225App() {

    var showSplash by remember { mutableStateOf(true) }

    if (showSplash) {
        SplashScreen {
            showSplash = false
        }
    } else {
        CourseListScreen()
    }
}

@Composable
fun SplashScreen(onTimeout: () -> Unit) {

    LaunchedEffect(Unit) {
        delay(3000)
        onTimeout()
    }

    Box(
        modifier = Modifier.fillMaxSize(),
        contentAlignment = Alignment.Center
    ) {
        Text(
            text = "CA1 CSE225",
            style = MaterialTheme.typography.headlineLarge
        )
    }
}

data class Course(
    val name: String,
    val duration: String,
    val rating: Int
)

val courseList = listOf(
    Course("Android Development", "3 Months", 5),
    Course("Cyber Security", "4 Months", 4),
    Course("Data Science", "6 Months", 5),
    Course("Web Development", "5 Months", 3),
    Course("AI & ML", "6 Months", 4)
)

@Composable
fun CourseListScreen() {

    var loading by remember { mutableStateOf(true) }

    LaunchedEffect(Unit) {
        delay(2000)
        loading = false
    }

    if (loading) {
        Box(
            modifier = Modifier.fillMaxSize(),
            contentAlignment = Alignment.Center
        ) {
            CircularProgressIndicator()
        }
    } else {
        LazyColumn(
            modifier = Modifier
                .fillMaxSize()
                .padding(16.dp)
        ) {
            items(courseList) { course ->
                CourseItem(course)
            }
        }
    }
}

@Composable
fun CourseItem(course: Course) {

    Card(
        modifier = Modifier
            .fillMaxWidth()
            .padding(vertical = 8.dp)
    ) {
        Column(
            modifier = Modifier.padding(16.dp)
        ) {
            Text(
                text = course.name,
                style = MaterialTheme.typography.titleLarge
            )

            Text(text = "Duration: ${course.duration}")

            RatingBar(rating = course.rating)
        }
    }
}

@Composable
fun RatingBar(rating: Int) {
    Row {
        for (i in 1..5) {
            if (i <= rating)
                Text("⭐")
            else
                Text("☆")
        }
    }
}

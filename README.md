# MTCARS
Final Project Week 12
library(shiny)
library(ggplot2)
library(dplyr)


data(mtcars)


ui <- fluidPage(
  titlePanel("Interactive Cars Explorer"),
  sidebarLayout(
    sidebarPanel(
      h4("Select Visualization Options:"),
      selectInput("x_variable", "X-Axis Variable:", names(mtcars), selected = "mpg"),
      selectInput("y_variable", "Y-Axis Variable:", names(mtcars), selected = "hp"),
      sliderInput("point_size", "Point Size:", min = 1, max = 5, value = 3),
      br(),
      h5("About the App:"),
      p("Explore the relationship between different variables in the mtcars dataset."),
      p("Customize the X and Y axes, and adjust point size for a personalized visualization.")
    ),
    mainPanel(
      plotOutput("scatterplot"),
      br(),
      dataTableOutput("table")
    )
  )
)


server <- function(input, output) {

  reactive_data <- reactive({
    mtcars
  })
  

  output$scatterplot <- renderPlot({
    ggplot(reactive_data(), aes_string(x = input$x_variable, y = input$y_variable)) +
      geom_point(size = input$point_size, color = "blue") +
      labs(title = paste("Scatter Plot of", input$y_variable, "vs.", input$x_variable),
           x = input$x_variable, y = input$y_variable) +
      theme_minimal()
  })
  

  output$table <- renderDataTable({
    reactive_data()
  })
}


shinyApp(ui, server)

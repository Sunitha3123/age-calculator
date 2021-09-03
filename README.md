# age-calculator
#This is my first program using the domain "shiny apps"(R language)
##https://sunithaboddupally.shinyapps.io/agecalc/

#code for the program
library(shiny)

server = function (input, output,session)
{
  
    datasetInput <- reactive({  
    age = age_calc(input$date3,enddate = Sys.Date(),units = "years",precise = TRUE)
    
    age = data.frame(age)
    names(age) = "AGE"
    print(age)
    
  })

 
 #Status/Output Text Box
 output$contents <- renderPrint({
   if (input$submitbutton>0) { 
     isolate("Calculation complete.") 
   } else {
     return("Server is ready for calculation.")
   }
  } )
 
 
 output$tabledata <- renderTable({
   if (input$submitbutton>0) { 
     isolate(datasetInput()) 
   } 
 })

 
}


ui = fluidPage( h1("AGE CALCULATOR"),
  navbarPage("AGE Calculator:",tabPanel("Home",
           # Input values
           sidebarPanel(
             HTML("<h3>Input parameters</h3>"),
             dateInput("date3", 
                         label = "DOB", 
                         value = "1998-05-31", 
                         
                         ),
             
 
  actionButton("submitbutton","Submit")
),

mainPanel(
  tags$label(h3('Output')), # Status/Output Text Box
  verbatimTextOutput('contents'),
  tableOutput('tabledata') # Results table
)

)))
  

shinyApp(ui=ui, server = server)










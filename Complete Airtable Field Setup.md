
You're right - I need to specify the exact field setup for the Discovery and Motions tables as well. Here are the complete field specifications for all four tables:

### Cases Table

1. Rename the default "Table 1" to "Cases"
    
2. Add these fields:
    
    - Case ID (Single line text) - for linking records
        
    - Case Name (Single line text)
        
    - Client Name (Single line text)
        
    - Case Type (Single select)
        
    - Phase (Single select) - with options like Pleadings, Discovery, Dispositive Motions, etc.
        
    - Venue (Single line text)
        
    - Next Due Date (Date)
        
    - Next Due Type (Single line text)
        
    - Next Notes (Long text)
        
    - Status (Single select) - with options like Active, Closed, Suspended
        
    - Case Health (Formula) - configured with the formula we created
        
    - Days Until Next Deadline (Formula) - as configured earlier
        

### Deadlines Table

1. Add a new table named "Deadlines"
    
2. Add these fields:
    
    - Deadline ID (Single line text)
        
    - Deadline Name (Single line text)
        
    - Case (Link to Cases table)
        
    - Deadline Type (Single select) - Trial, Discovery, Expert, Dispositive Motion, Mediation
        
    - Deadline Date (Date)
        
    - Status (Single select) - Upcoming, Completed, Missed
        
    - Notes (Long text)
        
    - Deadline Status (Formula) - configured earlier
        
    - Deadline Age (Formula) - configured earlier
        

### Discovery Table

1. Add a new table named "Discovery"
    
2. Add these fields:
    
    - Discovery ID (Single line text)
        
    - Discovery Item (Single line text)
        
    - Case (Link to Cases table)
        
    - Type (Single select) - Initial Disclosure, Discovery Package, etc.
        
    - Direction (Single select) - Sent, Received
        
    - Date Sent (Date)
        
    - Date Received (Date)
        
    - Status (Single select) - Complete, Pending, In Progress
        
    - Notes (Long text)
        

### Motions & Hearings Table

1. Add a new table named "Motions"
    
2. Add these fields:
    
    - Motion ID (Single line text)
        
    - Motion/Hearing Name (Single line text)
        
    - Case (Link to Cases table)
        
    - Type (Single select) - Motion, Hearing, Deposition, Mediation, Missing Pleadings
        
    - Filing Date (Date)
        
    - Hearing Date (Date)
        
    - Status (Single select) - Pending, Scheduled, Complete, Action Required
        
    - Notes (Long text)
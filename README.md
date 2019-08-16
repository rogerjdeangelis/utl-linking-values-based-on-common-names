# utl-linking-values-based-on-common-names
Linking values based on common names
    Linking values based on common names                                                                                                       
    Another Solution by KSharpe (diferent output slightly differeent assumptions?)
    https://communities.sas.com/t5/user/viewprofilepage/user-id/18408   
                                                                    
    PROC SORT                                                           
      DATA=WORK.have                                                
      OUT=WORK.SORTTempTableSorted                                  
      ;                                                             
      BY NAME FILE1       Status;                                   
    RUN;                                                                
    PROC TRANSPOSE DATA=WORK.SORTTempTableSorted                        
      OUT=TRNSTRANSPOSED                                            
      PREFIX=VALUE;                                                 
      BY NAME FILE1       Status;                                   
      VAR VALUE;                                                    
    RUN;                
                                                                                                                                               
    github                                                                                                                                     
    https://tinyurl.com/y4z82uz9                                                                                                               
    https://github.com/rogerjdeangelis/utl-linking-values-based-on-common-names                                                                
                                                                                                                                               
    SAS Forum                                                                                                                                  
    https://tinyurl.com/yx9odjcc                                                                                                               
    https://communities.sas.com/t5/SAS-Data-Management/Transpose-transformation-SAS-DI/m-p/580470                                              
                                                                                                                                               
    I think a HASH might be more efficient. HOH?                                                                                               
                                                                                                                                               
    *_                   _                                                                                                                     
    (_)_ __  _ __  _   _| |_                                                                                                                   
    | | '_ \| '_ \| | | | __|                                                                                                                  
    | | | | | |_) | |_| | |_                                                                                                                   
    |_|_| |_| .__/ \__,_|\__|                                                                                                                  
            |_|                                                                                                                                
    ;                                                                                                                                          
                                                                                                                                               
    data have;                                                                                                                                 
      infile cards4 delimiter="," dsd;                                                                                                         
      informat Value NAME FILE1 Status $24.;                                                                                                   
      input Value NAME FILE1 Status;                                                                                                           
    cards4;                                                                                                                                    
    Value,NAME,FILE1,Status                                                                                                                    
    SSSSS,567Q44104,DADA,Not Applicable                                                                                                        
    SSSSS,20726910,,Accepted                                                                                                                   
    Banking,567Q44104,DADA,Not Applicable                                                                                                      
    AAG Portf,567Q44104,DADA,Not Applicable                                                                                                    
    AAG Portf,S68855kl,,Not Applicable                                                                                                         
    Failed,S68855kl,,Not Applicable                                                                                                            
    Cache,S68855kl,,Not Applicable                                                                                                             
    AAG Txn,567Q44104,DADA,Not Applicable                                                                                                      
    AAG Txn,S68855kl,,Not Applicable                                                                                                           
    AAG Txn,20726910,,Accepted                                                                                                                 
    ADF,20718435,,Accepted                                                                                                                     
    ADG,20726910,,Accepted                                                                                                                     
    CXE,S68855kl,,Not Applicable                                                                                                               
    ;;;;                                                                                                                                       
    run;quit;                                                                                                                                  
                                                                                                                                               
                                                                                                                                               
                                                                                                                                               
    WORK.HAVE total obs=14                               | RULES:                                                                              
                                                          |                                                                                    
      VALUE        NAME         FILE1    STATUS           |                                                                                    
                                                          |                                                                                    
      Value        NAME         FILE1    Status           |  VALUE    NAME     FILE1  STATUS          COL1       COL2        COL3              
                                                                                                                                               
      SSSSS        567Q44104    DADA     Not Applicable   |  SSSSS  567Q44104  DADA   Not Applicable  AAG Portf  Banking   AAG Txn             
      SSSSS        20726910              Accepted         |  SSSSS  20726910          Accepted        ADG        AAG Txn                       
                                                                                                                                               
                                                                LOOK UP this triad of variables                                                
                                                                                                                                               
                                                                 NAME       FILE1    STATUS              Values for the same triad             
                                                                ------------------------------------     --------------------------            
                                                                567Q44104    DADA     Not Applicable      AAG Portf                            
                                                                                                          Banking                              
                                                                                                          AAG Txn                              
                                                                                                                                               
      Banking      567Q44104    DADA     Not Applicable   |                                                                                    
      AAG Portf    567Q44104    DADA     Not Applicable   |                                                                                    
      AAG Portf    S68855kl              Not Applicable   |                                                                                    
      Failed       S68855kl              Not Applicable   |                                                                                    
      Cache        S68855kl              Not Applicable   |                                                                                    
      AAG Txn      567Q44104    DADA     Not Applicable   |                                                                                    
      AAG Txn      S68855kl              Not Applicable   |                                                                                    
      AAG Txn      20726910              Accepted         |                                                                                    
      ADF          20718435              Accepted         |                                                                                    
      ADG          20726910              Accepted         |                                                                                    
      CXE          S68855kl              Not Applicable   |                                                                                    
                                                                                                                                               
    *            _               _                                                                                                             
      ___  _   _| |_ _ __  _   _| |_                                                                                                           
     / _ \| | | | __| '_ \| | | | __|                                                                                                          
    | (_) | |_| | |_| |_) | |_| | |_                                                                                                           
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                                          
                    |_|                                                                                                                        
    ;                                                                                                                                          
                                                                                                                                               
    LV           NAME       FILE1    STATUS            COL1         COL2           COL3         COL4                                           
                                                                                                                                               
    AAG Txn    567Q44104    DADA     Not Applicable    AAG Portf                                                                               
    AAG Txn    S68855kl              Not Applicable    AAG Portf                                                                               
    ADG        20726910              Accepted          AAG Txn                                                                                 
    Banking    567Q44104    DADA     Not Applicable    AAG Portf    AAG Txn                                                                    
    CXE        S68855kl              Not Applicable    AAG Txn      AAG Portf                                                                  
    Cache      S68855kl              Not Applicable    AAG Txn      CXE          AAG Portf                                                     
    Failed     S68855kl              Not Applicable    CXE          Cache        AAG Txn      AAG Portf                                        
    SSSSS      20726910              Accepted          ADG          AAG Txn                                                                    
    SSSSS      567Q44104    DADA     Not Applicable    AAG Portf    Banking      AAG Txn                                                       
                                                                                                                                               
    *          _       _   _                                                                                                                   
     ___  ___ | |_   _| |_(_) ___  _ __                                                                                                        
    / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                                                       
    \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                                      
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                                      
                                                                                                                                               
    ;                                                                                                                                          
                                                                                                                                               
    proc sql;                                                                                                                                  
      create                                                                                                                                   
          table havJyn as                                                                                                                      
      select                                                                                                                                   
          l.value  as lv                                                                                                                       
         ,r.value  as rv                                                                                                                       
         ,r.name                                                                                                                               
         ,r.file1                                                                                                                              
         ,r.status                                                                                                                             
      from                                                                                                                                     
         have as l left join have as r                                                                                                         
      on                                                                                                                                       
         r.name   = l.name     and                                                                                                             
         r.file1  = l.file1    and                                                                                                             
         r.status = l.status   and                                                                                                             
         l.value  > r.value                                                                                                                    
      order                                                                                                                                    
         by                                                                                                                                    
            l.value                                                                                                                            
           ,r.name                                                                                                                             
           ,r.file1                                                                                                                            
           ,r.status                                                                                                                           
    ;quit;                                                                                                                                     
                                                                                                                                               
                                                                                                                                               
    proc transpose data=havjyn out=havxpo(drop=_name_ where=(col1 ne ""));                                                                     
    by lv name       file1    status;                                                                                                          
    var rv;                                                                                                                                    
    run;quit;                                                                                                                                  
                                                                                                                                               
                                                                                                                                               

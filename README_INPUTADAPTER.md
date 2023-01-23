# OpenLoopInputAdapter

OpenLoopInputAdapter is the entry point in our pipeline for afp letters which come from Inspire. 
Its role is to normalize the TLEs in the afp files we receive according the requirements. To achieve that, the application uses classes called "Operators" 
that run through all the Structured Fields in an AFP file and perform transformations. 
These operators can be found in the package `ing.openloop.inputadapter.operator`. They perform actions such as:

 - CheckAfpOperator
   * Operator that calls CheckAfpState to check the AFP.

 - ReplaceEmptyDomainOperator
   * If the TLE field DOMAIN is empty, file name is rewritten with prefix "UNK" (unknown).

 - CheckTleOperator
   * Operator that checks if all documents fulfill the `TLERequirements#getStandardTLERequirements()`.

 - RemoveNopOperator
   * Operator that discards all NOP.

 - DefaultPaperEnvelopeOperator
    - DefaultPaperEnvelopeOperatorBe
       * Set envelope type and paper type for Belgian entities.
    - DefaultPaperEnvelopeOperatorNl
       * Set envelope type and paper type for Dutch entities.

 - ColorMappingOperator
   * Operator that changes the colors in GAD and PTX for Dutch entity letters using the rules of the ColorMappingRepository.
 
 - AddForceFrontNopOperator
   * Operator that adds a 'SubDocumentSeparator' NOP before the page (between EPG and BPG);
   * if the PCSP_FORCE_FRONT_IND TLE has been found with the correct value;
   * It also buffers entire pages (from EPG to BPG) to make this possible.

 - FillSubtypeOperator
   * Operator that fills the Subtype so that OpenLoopRelocate can read it.

 - PlexOperator
   * Add PLEX TLE field, which determines whether a letter is simplex or duplex.
 
 - NewFileNameOperator
   * Rewrites the file name with a prefix from the TLE field "DOMAIN". 

Some operators depend on the result of the action from other operators. Therefore, a priority list is defined for 
the order in which the operators should run. This list can be found in the class `ing.openloop.inputadapter.runcontrol.InputSource`.

A status extension is added to the files that are or have been processed by OpenLoopInputAdapter. Those status are: _.tmp, .busy, .done, and .error_. 
They are defined in the class `ing.openloop.inputadapter.runcontrol.InputAdapterJobExecutor`.
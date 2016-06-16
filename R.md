R
=

## Basic commands

    attributes(object)

    mode(object)

    ls()

    rm(object)

    length(object)              ... displays number of columns

    which(object, regexp)       ... displays true or false

    which.min(object)

    which.max(object)

    rownames(object)            ... displays row names

    which.max(rownames(object)) ... displays number of rows

    matrix_name[,1]             ... display column 1
    matrix_name[1,]             ... display row 1
    matrix_name[1,1]            ... display value at column 1, row 1

    paste("A", "B", "C", sep="")... concatenate string

    grepl(pattern, vector)      ... search pattern suchen, returns true or false
    sub, gsub                   ... regexp replace, sub replaces first regexp occurrence,
                                            gsub replaces all regexp occurrences

    attach(matrix_name)         ... if column names of a matrix exist, attach enables their usage as variables
    detach(matrix_name)         ... removes attached column names

    sub(", ", "", paste(toString(levels(l_readMSAQuick[1,4])), sep="", collapse=NULL))
                                ... display different levels as string

    strsplit([stringToSplit], splitparameter)

    nchar(string)               ... returns lenght of a string

    substr(x, start, stop)

    unique(array|matrixcolumn)  ... returns array with all unique values of the array or selected matrix column.
        e.g. unique(m_test[1:30,1])

    length(unique(array|matrixcolumn))
                                ... returns the number of all unique values of the array or selected column.

    rep("x", 3)                 ... value x is repeated 3 times and returned in an array of length 3.

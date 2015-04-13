\begin{Exercise}[title={Interfaces and compilation},difficulty=1]
\Question
The code in listing (#src:interface fail) on page
\pageref{src:interface fail} compiles OK --- as stated 
in the text. But when you run it you'll get a runtime error, so
something *is* wrong. Why does the code compile cleanly then?
\end{Exercise}

\begin{Answer}
\Question
The code compiles because an integer type implements the empty interface
and that is the check that happens at compile time.

A proper way to fix this is to test if such an empty interface can
be converted and, if so, call the appropriate method. The Go code
that defines the function `g` in listing (#src:interface empty)
-- repeated here:
\begin{lstlisting}
func g(any interface{}) int { return any.(I).Get() }
\end{lstlisting}

Should be changed to become:
\begin{lstlisting}
func g(any interface{}) int {
    if v, ok := any.(I); ok {	|\coderemark{Check if any can be converted}|
        return v.Get()		|\coderemark{If so invoke Get()}|
    }
    return -1			|\coderemark{Just so we return anything}|
}
\end{lstlisting}
If `g()` is called now there are no run-time errors anymore. The
idiom used is called "comma ok" in Go.
\end{Answer}
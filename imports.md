<h1><a id="imports"></a>World imports</h1>
<ul>
<li>Imports:
<ul>
<li>interface <a href="#wasi_io_error_0_2_2"><code>wasi:io/error@0.2.2</code></a></li>
<li>interface <a href="#wasi_io_poll_0_2_2"><code>wasi:io/poll@0.2.2</code></a></li>
<li>interface <a href="#wasi_io_streams_0_2_2"><code>wasi:io/streams@0.2.2</code></a></li>
<li>interface <a href="#wasi_blobstore_types_0_2_0_draft"><code>wasi:blobstore/types@0.2.0-draft</code></a></li>
<li>interface <a href="#wasi_blobstore_container_0_2_0_draft"><code>wasi:blobstore/container@0.2.0-draft</code></a></li>
<li>interface <a href="#wasi_blobstore_blobstore_0_2_0_draft"><code>wasi:blobstore/blobstore@0.2.0-draft</code></a></li>
</ul>
</li>
</ul>
<h2><a id="wasi_io_error_0_2_2"></a>Import interface wasi:io/error@0.2.2</h2>
<hr />
<h3>Types</h3>
<h4><a id="error"></a><code>resource error</code></h4>
<p>A resource which represents some error information.</p>
<p>The only method provided by this resource is <code>to-debug-string</code>,
which provides some human-readable information about the error.</p>
<p>In the <code>wasi:io</code> package, this resource is returned through the
<code>wasi:io/streams/stream-error</code> type.</p>
<p>To provide more specific error information, other interfaces may
offer functions to &quot;downcast&quot; this error into more specific types. For example,
errors returned from streams derived from filesystem types can be described using
the filesystem's own error-code type. This is done using the function
<code>wasi:filesystem/types/filesystem-error-code</code>, which takes a <code>borrow&lt;error&gt;</code>
parameter and returns an <code>option&lt;wasi:filesystem/types/error-code&gt;</code>.</p>
<h2>The set of functions which can &quot;downcast&quot; an <a href="#error"><code>error</code></a> into a more
concrete type is open.</h2>
<h3>Functions</h3>
<h4><a id="method_error_to_debug_string"></a><code>[method]error.to-debug-string: func</code></h4>
<p>Returns a string that is suitable to assist humans in debugging
this error.</p>
<p>WARNING: The returned string should not be consumed mechanically!
It may change across platforms, hosts, or other implementation
details. Parsing this string is a major platform-compatibility
hazard.</p>
<h5>Params</h5>
<ul>
<li><a id="method_error_to_debug_string.self"></a><code>self</code>: borrow&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_error_to_debug_string.0"></a> <code>string</code></li>
</ul>
<h2><a id="wasi_io_poll_0_2_2"></a>Import interface wasi:io/poll@0.2.2</h2>
<p>A poll API intended to let users wait for I/O events on multiple handles
at once.</p>
<hr />
<h3>Types</h3>
<h4><a id="pollable"></a><code>resource pollable</code></h4>
<h2><a href="#pollable"><code>pollable</code></a> represents a single I/O event which may be ready, or not.</h2>
<h3>Functions</h3>
<h4><a id="method_pollable_ready"></a><code>[method]pollable.ready: func</code></h4>
<p>Return the readiness of a pollable. This function never blocks.</p>
<p>Returns <code>true</code> when the pollable is ready, and <code>false</code> otherwise.</p>
<h5>Params</h5>
<ul>
<li><a id="method_pollable_ready.self"></a><code>self</code>: borrow&lt;<a href="#pollable"><a href="#pollable"><code>pollable</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_pollable_ready.0"></a> <code>bool</code></li>
</ul>
<h4><a id="method_pollable_block"></a><code>[method]pollable.block: func</code></h4>
<p><code>block</code> returns immediately if the pollable is ready, and otherwise
blocks until ready.</p>
<p>This function is equivalent to calling <code>poll.poll</code> on a list
containing only this pollable.</p>
<h5>Params</h5>
<ul>
<li><a id="method_pollable_block.self"></a><code>self</code>: borrow&lt;<a href="#pollable"><a href="#pollable"><code>pollable</code></a></a>&gt;</li>
</ul>
<h4><a id="poll"></a><code>poll: func</code></h4>
<p>Poll for completion on a set of pollables.</p>
<p>This function takes a list of pollables, which identify I/O sources of
interest, and waits until one or more of the events is ready for I/O.</p>
<p>The result <code>list&lt;u32&gt;</code> contains one or more indices of handles in the
argument list that is ready for I/O.</p>
<p>This function traps if either:</p>
<ul>
<li>the list is empty, or:</li>
<li>the list contains more elements than can be indexed with a <code>u32</code> value.</li>
</ul>
<p>A timeout can be implemented by adding a pollable from the
wasi-clocks API to the list.</p>
<p>This function does not return a <code>result</code>; polling in itself does not
do any I/O so it doesn't fail. If any of the I/O sources identified by
the pollables has an error, it is indicated by marking the source as
being ready for I/O.</p>
<h5>Params</h5>
<ul>
<li><a id="poll.in"></a><code>in</code>: list&lt;borrow&lt;<a href="#pollable"><a href="#pollable"><code>pollable</code></a></a>&gt;&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="poll.0"></a> list&lt;<code>u32</code>&gt;</li>
</ul>
<h2><a id="wasi_io_streams_0_2_2"></a>Import interface wasi:io/streams@0.2.2</h2>
<p>WASI I/O is an I/O abstraction API which is currently focused on providing
stream types.</p>
<p>In the future, the component model is expected to add built-in stream types;
when it does, they are expected to subsume this API.</p>
<hr />
<h3>Types</h3>
<h4><a id="error"></a><code>type error</code></h4>
<p><a href="#error"><a href="#error"><code>error</code></a></a></p>
<p>
#### <a id="pollable"></a>`type pollable`
[`pollable`](#pollable)
<p>
#### <a id="stream_error"></a>`variant stream-error`
<p>An error for input-stream and output-stream operations.</p>
<h5>Variant Cases</h5>
<ul>
<li>
<p><a id="stream_error.last_operation_failed"></a><code>last-operation-failed</code>: own&lt;<a href="#error"><a href="#error"><code>error</code></a></a>&gt;</p>
<p>The last operation (a write or flush) failed before completion.
<p>More information is available in the <a href="#error"><code>error</code></a> payload.</p>
<p>After this, the stream will be closed. All future operations return
<a href="#stream_error.closed"><code>stream-error::closed</code></a>.</p>
</li>
<li>
<p><a id="stream_error.closed"></a><code>closed</code></p>
<p>The stream is closed: no more input will be accepted by the
stream. A closed output-stream will return this error on all
future operations.
</li>
</ul>
<h4><a id="input_stream"></a><code>resource input-stream</code></h4>
<p>An input bytestream.</p>
<p><a href="#input_stream"><code>input-stream</code></a>s are <em>non-blocking</em> to the extent practical on underlying
platforms. I/O operations always return promptly; if fewer bytes are
promptly available than requested, they return the number of bytes promptly
available, which could even be zero. To wait for data to be available,
use the <code>subscribe</code> function to obtain a <a href="#pollable"><code>pollable</code></a> which can be polled
for using <code>wasi:io/poll</code>.</p>
<h4><a id="output_stream"></a><code>resource output-stream</code></h4>
<p>An output bytestream.</p>
<p><a href="#output_stream"><code>output-stream</code></a>s are <em>non-blocking</em> to the extent practical on
underlying platforms. Except where specified otherwise, I/O operations also
always return promptly, after the number of bytes that can be written
promptly, which could even be zero. To wait for the stream to be ready to
accept data, the <code>subscribe</code> function to obtain a <a href="#pollable"><code>pollable</code></a> which can be
polled for using <code>wasi:io/poll</code>.</p>
<h2>Dropping an <a href="#output_stream"><code>output-stream</code></a> while there's still an active write in
progress may result in the data being lost. Before dropping the stream,
be sure to fully flush your writes.</h2>
<h3>Functions</h3>
<h4><a id="method_input_stream_read"></a><code>[method]input-stream.read: func</code></h4>
<p>Perform a non-blocking read from the stream.</p>
<p>When the source of a <code>read</code> is binary data, the bytes from the source
are returned verbatim. When the source of a <code>read</code> is known to the
implementation to be text, bytes containing the UTF-8 encoding of the
text are returned.</p>
<p>This function returns a list of bytes containing the read data,
when successful. The returned list will contain up to <code>len</code> bytes;
it may return fewer than requested, but not more. The list is
empty when no bytes are available for reading at this time. The
pollable given by <code>subscribe</code> will be ready when more bytes are
available.</p>
<p>This function fails with a <a href="#stream_error"><code>stream-error</code></a> when the operation
encounters an error, giving <code>last-operation-failed</code>, or when the
stream is closed, giving <code>closed</code>.</p>
<p>When the caller gives a <code>len</code> of 0, it represents a request to
read 0 bytes. If the stream is still open, this call should
succeed and return an empty list, or otherwise fail with <code>closed</code>.</p>
<p>The <code>len</code> parameter is a <code>u64</code>, which could represent a list of u8 which
is not possible to allocate in wasm32, or not desirable to allocate as
as a return value by the callee. The callee may return a list of bytes
less than <code>len</code> in size while more bytes are available for reading.</p>
<h5>Params</h5>
<ul>
<li><a id="method_input_stream_read.self"></a><code>self</code>: borrow&lt;<a href="#input_stream"><a href="#input_stream"><code>input-stream</code></a></a>&gt;</li>
<li><a id="method_input_stream_read.len"></a><code>len</code>: <code>u64</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_input_stream_read.0"></a> result&lt;list&lt;<code>u8</code>&gt;, <a href="#stream_error"><a href="#stream_error"><code>stream-error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_input_stream_blocking_read"></a><code>[method]input-stream.blocking-read: func</code></h4>
<p>Read bytes from a stream, after blocking until at least one byte can
be read. Except for blocking, behavior is identical to <code>read</code>.</p>
<h5>Params</h5>
<ul>
<li><a id="method_input_stream_blocking_read.self"></a><code>self</code>: borrow&lt;<a href="#input_stream"><a href="#input_stream"><code>input-stream</code></a></a>&gt;</li>
<li><a id="method_input_stream_blocking_read.len"></a><code>len</code>: <code>u64</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_input_stream_blocking_read.0"></a> result&lt;list&lt;<code>u8</code>&gt;, <a href="#stream_error"><a href="#stream_error"><code>stream-error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_input_stream_skip"></a><code>[method]input-stream.skip: func</code></h4>
<p>Skip bytes from a stream. Returns number of bytes skipped.</p>
<p>Behaves identical to <code>read</code>, except instead of returning a list
of bytes, returns the number of bytes consumed from the stream.</p>
<h5>Params</h5>
<ul>
<li><a id="method_input_stream_skip.self"></a><code>self</code>: borrow&lt;<a href="#input_stream"><a href="#input_stream"><code>input-stream</code></a></a>&gt;</li>
<li><a id="method_input_stream_skip.len"></a><code>len</code>: <code>u64</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_input_stream_skip.0"></a> result&lt;<code>u64</code>, <a href="#stream_error"><a href="#stream_error"><code>stream-error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_input_stream_blocking_skip"></a><code>[method]input-stream.blocking-skip: func</code></h4>
<p>Skip bytes from a stream, after blocking until at least one byte
can be skipped. Except for blocking behavior, identical to <code>skip</code>.</p>
<h5>Params</h5>
<ul>
<li><a id="method_input_stream_blocking_skip.self"></a><code>self</code>: borrow&lt;<a href="#input_stream"><a href="#input_stream"><code>input-stream</code></a></a>&gt;</li>
<li><a id="method_input_stream_blocking_skip.len"></a><code>len</code>: <code>u64</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_input_stream_blocking_skip.0"></a> result&lt;<code>u64</code>, <a href="#stream_error"><a href="#stream_error"><code>stream-error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_input_stream_subscribe"></a><code>[method]input-stream.subscribe: func</code></h4>
<p>Create a <a href="#pollable"><code>pollable</code></a> which will resolve once either the specified stream
has bytes available to read or the other end of the stream has been
closed.
The created <a href="#pollable"><code>pollable</code></a> is a child resource of the <a href="#input_stream"><code>input-stream</code></a>.
Implementations may trap if the <a href="#input_stream"><code>input-stream</code></a> is dropped before
all derived <a href="#pollable"><code>pollable</code></a>s created with this function are dropped.</p>
<h5>Params</h5>
<ul>
<li><a id="method_input_stream_subscribe.self"></a><code>self</code>: borrow&lt;<a href="#input_stream"><a href="#input_stream"><code>input-stream</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_input_stream_subscribe.0"></a> own&lt;<a href="#pollable"><a href="#pollable"><code>pollable</code></a></a>&gt;</li>
</ul>
<h4><a id="method_output_stream_check_write"></a><code>[method]output-stream.check-write: func</code></h4>
<p>Check readiness for writing. This function never blocks.</p>
<p>Returns the number of bytes permitted for the next call to <code>write</code>,
or an error. Calling <code>write</code> with more bytes than this function has
permitted will trap.</p>
<p>When this function returns 0 bytes, the <code>subscribe</code> pollable will
become ready when this function will report at least 1 byte, or an
error.</p>
<h5>Params</h5>
<ul>
<li><a id="method_output_stream_check_write.self"></a><code>self</code>: borrow&lt;<a href="#output_stream"><a href="#output_stream"><code>output-stream</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_output_stream_check_write.0"></a> result&lt;<code>u64</code>, <a href="#stream_error"><a href="#stream_error"><code>stream-error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_output_stream_write"></a><code>[method]output-stream.write: func</code></h4>
<p>Perform a write. This function never blocks.</p>
<p>When the destination of a <code>write</code> is binary data, the bytes from
<code>contents</code> are written verbatim. When the destination of a <code>write</code> is
known to the implementation to be text, the bytes of <code>contents</code> are
transcoded from UTF-8 into the encoding of the destination and then
written.</p>
<p>Precondition: check-write gave permit of Ok(n) and contents has a
length of less than or equal to n. Otherwise, this function will trap.</p>
<p>returns Err(closed) without writing if the stream has closed since
the last call to check-write provided a permit.</p>
<h5>Params</h5>
<ul>
<li><a id="method_output_stream_write.self"></a><code>self</code>: borrow&lt;<a href="#output_stream"><a href="#output_stream"><code>output-stream</code></a></a>&gt;</li>
<li><a id="method_output_stream_write.contents"></a><code>contents</code>: list&lt;<code>u8</code>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_output_stream_write.0"></a> result&lt;_, <a href="#stream_error"><a href="#stream_error"><code>stream-error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_output_stream_blocking_write_and_flush"></a><code>[method]output-stream.blocking-write-and-flush: func</code></h4>
<p>Perform a write of up to 4096 bytes, and then flush the stream. Block
until all of these operations are complete, or an error occurs.</p>
<p>This is a convenience wrapper around the use of <code>check-write</code>,
<code>subscribe</code>, <code>write</code>, and <code>flush</code>, and is implemented with the
following pseudo-code:</p>
<pre><code class="language-text">let pollable = this.subscribe();
while !contents.is_empty() {
  // Wait for the stream to become writable
  pollable.block();
  let Ok(n) = this.check-write(); // eliding error handling
  let len = min(n, contents.len());
  let (chunk, rest) = contents.split_at(len);
  this.write(chunk  );            // eliding error handling
  contents = rest;
}
this.flush();
// Wait for completion of `flush`
pollable.block();
// Check for any errors that arose during `flush`
let _ = this.check-write();         // eliding error handling
</code></pre>
<h5>Params</h5>
<ul>
<li><a id="method_output_stream_blocking_write_and_flush.self"></a><code>self</code>: borrow&lt;<a href="#output_stream"><a href="#output_stream"><code>output-stream</code></a></a>&gt;</li>
<li><a id="method_output_stream_blocking_write_and_flush.contents"></a><code>contents</code>: list&lt;<code>u8</code>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_output_stream_blocking_write_and_flush.0"></a> result&lt;_, <a href="#stream_error"><a href="#stream_error"><code>stream-error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_output_stream_flush"></a><code>[method]output-stream.flush: func</code></h4>
<p>Request to flush buffered output. This function never blocks.</p>
<p>This tells the output-stream that the caller intends any buffered
output to be flushed. the output which is expected to be flushed
is all that has been passed to <code>write</code> prior to this call.</p>
<p>Upon calling this function, the <a href="#output_stream"><code>output-stream</code></a> will not accept any
writes (<code>check-write</code> will return <code>ok(0)</code>) until the flush has
completed. The <code>subscribe</code> pollable will become ready when the
flush has completed and the stream can accept more writes.</p>
<h5>Params</h5>
<ul>
<li><a id="method_output_stream_flush.self"></a><code>self</code>: borrow&lt;<a href="#output_stream"><a href="#output_stream"><code>output-stream</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_output_stream_flush.0"></a> result&lt;_, <a href="#stream_error"><a href="#stream_error"><code>stream-error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_output_stream_blocking_flush"></a><code>[method]output-stream.blocking-flush: func</code></h4>
<p>Request to flush buffered output, and block until flush completes
and stream is ready for writing again.</p>
<h5>Params</h5>
<ul>
<li><a id="method_output_stream_blocking_flush.self"></a><code>self</code>: borrow&lt;<a href="#output_stream"><a href="#output_stream"><code>output-stream</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_output_stream_blocking_flush.0"></a> result&lt;_, <a href="#stream_error"><a href="#stream_error"><code>stream-error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_output_stream_subscribe"></a><code>[method]output-stream.subscribe: func</code></h4>
<p>Create a <a href="#pollable"><code>pollable</code></a> which will resolve once the output-stream
is ready for more writing, or an error has occurred. When this
pollable is ready, <code>check-write</code> will return <code>ok(n)</code> with n&gt;0, or an
error.</p>
<p>If the stream is closed, this pollable is always ready immediately.</p>
<p>The created <a href="#pollable"><code>pollable</code></a> is a child resource of the <a href="#output_stream"><code>output-stream</code></a>.
Implementations may trap if the <a href="#output_stream"><code>output-stream</code></a> is dropped before
all derived <a href="#pollable"><code>pollable</code></a>s created with this function are dropped.</p>
<h5>Params</h5>
<ul>
<li><a id="method_output_stream_subscribe.self"></a><code>self</code>: borrow&lt;<a href="#output_stream"><a href="#output_stream"><code>output-stream</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_output_stream_subscribe.0"></a> own&lt;<a href="#pollable"><a href="#pollable"><code>pollable</code></a></a>&gt;</li>
</ul>
<h4><a id="method_output_stream_write_zeroes"></a><code>[method]output-stream.write-zeroes: func</code></h4>
<p>Write zeroes to a stream.</p>
<p>This should be used precisely like <code>write</code> with the exact same
preconditions (must use check-write first), but instead of
passing a list of bytes, you simply pass the number of zero-bytes
that should be written.</p>
<h5>Params</h5>
<ul>
<li><a id="method_output_stream_write_zeroes.self"></a><code>self</code>: borrow&lt;<a href="#output_stream"><a href="#output_stream"><code>output-stream</code></a></a>&gt;</li>
<li><a id="method_output_stream_write_zeroes.len"></a><code>len</code>: <code>u64</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_output_stream_write_zeroes.0"></a> result&lt;_, <a href="#stream_error"><a href="#stream_error"><code>stream-error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_output_stream_blocking_write_zeroes_and_flush"></a><code>[method]output-stream.blocking-write-zeroes-and-flush: func</code></h4>
<p>Perform a write of up to 4096 zeroes, and then flush the stream.
Block until all of these operations are complete, or an error
occurs.</p>
<p>This is a convenience wrapper around the use of <code>check-write</code>,
<code>subscribe</code>, <code>write-zeroes</code>, and <code>flush</code>, and is implemented with
the following pseudo-code:</p>
<pre><code class="language-text">let pollable = this.subscribe();
while num_zeroes != 0 {
  // Wait for the stream to become writable
  pollable.block();
  let Ok(n) = this.check-write(); // eliding error handling
  let len = min(n, num_zeroes);
  this.write-zeroes(len);         // eliding error handling
  num_zeroes -= len;
}
this.flush();
// Wait for completion of `flush`
pollable.block();
// Check for any errors that arose during `flush`
let _ = this.check-write();         // eliding error handling
</code></pre>
<h5>Params</h5>
<ul>
<li><a id="method_output_stream_blocking_write_zeroes_and_flush.self"></a><code>self</code>: borrow&lt;<a href="#output_stream"><a href="#output_stream"><code>output-stream</code></a></a>&gt;</li>
<li><a id="method_output_stream_blocking_write_zeroes_and_flush.len"></a><code>len</code>: <code>u64</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_output_stream_blocking_write_zeroes_and_flush.0"></a> result&lt;_, <a href="#stream_error"><a href="#stream_error"><code>stream-error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_output_stream_splice"></a><code>[method]output-stream.splice: func</code></h4>
<p>Read from one stream and write to another.</p>
<p>The behavior of splice is equivalent to:</p>
<ol>
<li>calling <code>check-write</code> on the <a href="#output_stream"><code>output-stream</code></a></li>
<li>calling <code>read</code> on the <a href="#input_stream"><code>input-stream</code></a> with the smaller of the
<code>check-write</code> permitted length and the <code>len</code> provided to <code>splice</code></li>
<li>calling <code>write</code> on the <a href="#output_stream"><code>output-stream</code></a> with that read data.</li>
</ol>
<p>Any error reported by the call to <code>check-write</code>, <code>read</code>, or
<code>write</code> ends the splice and reports that error.</p>
<p>This function returns the number of bytes transferred; it may be less
than <code>len</code>.</p>
<h5>Params</h5>
<ul>
<li><a id="method_output_stream_splice.self"></a><code>self</code>: borrow&lt;<a href="#output_stream"><a href="#output_stream"><code>output-stream</code></a></a>&gt;</li>
<li><a id="method_output_stream_splice.src"></a><code>src</code>: borrow&lt;<a href="#input_stream"><a href="#input_stream"><code>input-stream</code></a></a>&gt;</li>
<li><a id="method_output_stream_splice.len"></a><code>len</code>: <code>u64</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_output_stream_splice.0"></a> result&lt;<code>u64</code>, <a href="#stream_error"><a href="#stream_error"><code>stream-error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_output_stream_blocking_splice"></a><code>[method]output-stream.blocking-splice: func</code></h4>
<p>Read from one stream and write to another, with blocking.</p>
<p>This is similar to <code>splice</code>, except that it blocks until the
<a href="#output_stream"><code>output-stream</code></a> is ready for writing, and the <a href="#input_stream"><code>input-stream</code></a>
is ready for reading, before performing the <code>splice</code>.</p>
<h5>Params</h5>
<ul>
<li><a id="method_output_stream_blocking_splice.self"></a><code>self</code>: borrow&lt;<a href="#output_stream"><a href="#output_stream"><code>output-stream</code></a></a>&gt;</li>
<li><a id="method_output_stream_blocking_splice.src"></a><code>src</code>: borrow&lt;<a href="#input_stream"><a href="#input_stream"><code>input-stream</code></a></a>&gt;</li>
<li><a id="method_output_stream_blocking_splice.len"></a><code>len</code>: <code>u64</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_output_stream_blocking_splice.0"></a> result&lt;<code>u64</code>, <a href="#stream_error"><a href="#stream_error"><code>stream-error</code></a></a>&gt;</li>
</ul>
<h2><a id="wasi_blobstore_types_0_2_0_draft"></a>Import interface wasi:blobstore/types@0.2.0-draft</h2>
<p>Types used by blobstore</p>
<hr />
<h3>Types</h3>
<h4><a id="input_stream"></a><code>type input-stream</code></h4>
<p><a href="#input_stream"><a href="#input_stream"><code>input-stream</code></a></a></p>
<p>
#### <a id="output_stream"></a>`type output-stream`
[`output-stream`](#output_stream)
<p>
#### <a id="container_name"></a>`type container-name`
`string`
<p>name of a container, a collection of objects.
The container name may be any valid UTF-8 string.
<h4><a id="object_name"></a><code>type object-name</code></h4>
<p><code>string</code></p>
<p>name of an object within a container
The object name may be any valid UTF-8 string.
<h4><a id="timestamp"></a><code>type timestamp</code></h4>
<p><code>u64</code></p>
<p>TODO: define timestamp to include seconds since
Unix epoch and nanoseconds
https://github.com/WebAssembly/wasi-blob-store/issues/7
<h4><a id="object_size"></a><code>type object-size</code></h4>
<p><code>u64</code></p>
<p>size of an object, in bytes
<h4><a id="error"></a><code>type error</code></h4>
<p><code>string</code></p>
<p>
#### <a id="container_metadata"></a>`record container-metadata`
<p>information about a container</p>
<h5>Record Fields</h5>
<ul>
<li>
<p><a id="container_metadata.name"></a><code>name</code>: <a href="#container_name"><a href="#container_name"><code>container-name</code></a></a></p>
<p>the container's name
</li>
<li>
<p><a id="container_metadata.created_at"></a><code>created-at</code>: <a href="#timestamp"><a href="#timestamp"><code>timestamp</code></a></a></p>
<p>date and time container was created
</li>
</ul>
<h4><a id="object_metadata"></a><code>record object-metadata</code></h4>
<p>information about an object</p>
<h5>Record Fields</h5>
<ul>
<li>
<p><a id="object_metadata.name"></a><code>name</code>: <a href="#object_name"><a href="#object_name"><code>object-name</code></a></a></p>
<p>the object's name
</li>
<li>
<p><a id="object_metadata.container"></a><a href="#container"><code>container</code></a>: <a href="#container_name"><a href="#container_name"><code>container-name</code></a></a></p>
<p>the object's parent container
</li>
<li>
<p><a id="object_metadata.created_at"></a><code>created-at</code>: <a href="#timestamp"><a href="#timestamp"><code>timestamp</code></a></a></p>
<p>date and time the object was created
</li>
<li>
<p><a id="object_metadata.size"></a><code>size</code>: <a href="#object_size"><a href="#object_size"><code>object-size</code></a></a></p>
<p>size of the object, in bytes
</li>
</ul>
<h4><a id="object_id"></a><code>record object-id</code></h4>
<p>identifier for an object that includes its container name</p>
<h5>Record Fields</h5>
<ul>
<li><a id="object_id.container"></a><a href="#container"><code>container</code></a>: <a href="#container_name"><a href="#container_name"><code>container-name</code></a></a></li>
<li><a id="object_id.object"></a><code>object</code>: <a href="#object_name"><a href="#object_name"><code>object-name</code></a></a></li>
</ul>
<h4><a id="outgoing_value"></a><code>resource outgoing-value</code></h4>
<p>A data is the data stored in a data blob. The value can be of any type
that can be represented in a byte array. It provides a way to write the value
to the output-stream defined in the <code>wasi-io</code> interface.
Soon: switch to <code>resource value { ... }</code></p>
<h4><a id="incoming_value"></a><code>resource incoming-value</code></h4>
<p>A incoming-value is a wrapper around a value. It provides a way to read the value
from the input-stream defined in the <code>wasi-io</code> interface.</p>
<p>The incoming-value provides two ways to consume the value:</p>
<ol>
<li><code>incoming-value-consume-sync</code> consumes the value synchronously and returns the
value as a list of bytes.</li>
<li><code>incoming-value-consume-async</code> consumes the value asynchronously and returns the
value as an input-stream.
Soon: switch to <code>resource incoming-value { ... }</code></li>
</ol>
<h4><a id="incoming_value_async_body"></a><code>type incoming-value-async-body</code></h4>
<p><a href="#input_stream"><a href="#input_stream"><code>input-stream</code></a></a></p>
<p>
#### <a id="incoming_value_sync_body"></a>`type incoming-value-sync-body`
[`incoming-value-sync-body`](#incoming_value_sync_body)
<p>
----
<h3>Functions</h3>
<h4><a id="static_outgoing_value_new_outgoing_value"></a><code>[static]outgoing-value.new-outgoing-value: func</code></h4>
<h5>Return values</h5>
<ul>
<li><a id="static_outgoing_value_new_outgoing_value.0"></a> own&lt;<a href="#outgoing_value"><a href="#outgoing_value"><code>outgoing-value</code></a></a>&gt;</li>
</ul>
<h4><a id="method_outgoing_value_outgoing_value_write_body"></a><code>[method]outgoing-value.outgoing-value-write-body: func</code></h4>
<p>Returns a stream for writing the value contents.</p>
<p>The returned <a href="#output_stream"><code>output-stream</code></a> is a child resource: it must be dropped
before the parent <a href="#outgoing_value"><code>outgoing-value</code></a> resource is dropped (or finished),
otherwise the <a href="#outgoing_value"><code>outgoing-value</code></a> drop or <code>finish</code> will trap.</p>
<p>Returns success on the first call: the <a href="#output_stream"><code>output-stream</code></a> resource for
this <a href="#outgoing_value"><code>outgoing-value</code></a> may be retrieved at most once. Subsequent calls
will return error.</p>
<h5>Params</h5>
<ul>
<li><a id="method_outgoing_value_outgoing_value_write_body.self"></a><code>self</code>: borrow&lt;<a href="#outgoing_value"><a href="#outgoing_value"><code>outgoing-value</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_outgoing_value_outgoing_value_write_body.0"></a> result&lt;own&lt;<a href="#output_stream"><a href="#output_stream"><code>output-stream</code></a></a>&gt;&gt;</li>
</ul>
<h4><a id="static_outgoing_value_finish"></a><code>[static]outgoing-value.finish: func</code></h4>
<p>Finalize an outgoing value. This must be
called to signal that the outgoing value is complete. If the <a href="#outgoing_value"><code>outgoing-value</code></a>
is dropped without calling <code>outgoing-value.finalize</code>, the implementation
should treat the value as corrupted.</p>
<h5>Params</h5>
<ul>
<li><a id="static_outgoing_value_finish.this"></a><code>this</code>: own&lt;<a href="#outgoing_value"><a href="#outgoing_value"><code>outgoing-value</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="static_outgoing_value_finish.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="static_incoming_value_incoming_value_consume_sync"></a><code>[static]incoming-value.incoming-value-consume-sync: func</code></h4>
<h5>Params</h5>
<ul>
<li><a id="static_incoming_value_incoming_value_consume_sync.this"></a><code>this</code>: own&lt;<a href="#incoming_value"><a href="#incoming_value"><code>incoming-value</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="static_incoming_value_incoming_value_consume_sync.0"></a> result&lt;<a href="#incoming_value_sync_body"><a href="#incoming_value_sync_body"><code>incoming-value-sync-body</code></a></a>, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="static_incoming_value_incoming_value_consume_async"></a><code>[static]incoming-value.incoming-value-consume-async: func</code></h4>
<h5>Params</h5>
<ul>
<li><a id="static_incoming_value_incoming_value_consume_async.this"></a><code>this</code>: own&lt;<a href="#incoming_value"><a href="#incoming_value"><code>incoming-value</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="static_incoming_value_incoming_value_consume_async.0"></a> result&lt;own&lt;<a href="#incoming_value_async_body"><a href="#incoming_value_async_body"><code>incoming-value-async-body</code></a></a>&gt;, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_incoming_value_size"></a><code>[method]incoming-value.size: func</code></h4>
<h5>Params</h5>
<ul>
<li><a id="method_incoming_value_size.self"></a><code>self</code>: borrow&lt;<a href="#incoming_value"><a href="#incoming_value"><code>incoming-value</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_incoming_value_size.0"></a> <code>u64</code></li>
</ul>
<h2><a id="wasi_blobstore_container_0_2_0_draft"></a>Import interface wasi:blobstore/container@0.2.0-draft</h2>
<p>a Container is a collection of objects</p>
<hr />
<h3>Types</h3>
<h4><a id="input_stream"></a><code>type input-stream</code></h4>
<p><a href="#input_stream"><a href="#input_stream"><code>input-stream</code></a></a></p>
<p>
#### <a id="output_stream"></a>`type output-stream`
[`output-stream`](#output_stream)
<p>
#### <a id="container_metadata"></a>`type container-metadata`
[`container-metadata`](#container_metadata)
<p>
#### <a id="error"></a>`type error`
[`error`](#error)
<p>
#### <a id="incoming_value"></a>`type incoming-value`
[`incoming-value`](#incoming_value)
<p>
#### <a id="object_metadata"></a>`type object-metadata`
[`object-metadata`](#object_metadata)
<p>
#### <a id="object_name"></a>`type object-name`
[`object-name`](#object_name)
<p>
#### <a id="outgoing_value"></a>`type outgoing-value`
[`outgoing-value`](#outgoing_value)
<p>
#### <a id="container"></a>`resource container`
<p>this defines the <a href="#container"><code>container</code></a> resource</p>
<h4><a id="stream_object_names"></a><code>resource stream-object-names</code></h4>
<h2>this defines the <a href="#stream_object_names"><code>stream-object-names</code></a> resource which is a representation of stream<object-name></h2>
<h3>Functions</h3>
<h4><a id="method_container_name"></a><code>[method]container.name: func</code></h4>
<p>returns container name</p>
<h5>Params</h5>
<ul>
<li><a id="method_container_name.self"></a><code>self</code>: borrow&lt;<a href="#container"><a href="#container"><code>container</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_container_name.0"></a> result&lt;<code>string</code>, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_container_info"></a><code>[method]container.info: func</code></h4>
<p>returns container metadata</p>
<h5>Params</h5>
<ul>
<li><a id="method_container_info.self"></a><code>self</code>: borrow&lt;<a href="#container"><a href="#container"><code>container</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_container_info.0"></a> result&lt;<a href="#container_metadata"><a href="#container_metadata"><code>container-metadata</code></a></a>, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_container_get_data"></a><code>[method]container.get-data: func</code></h4>
<p>retrieves an object or portion of an object, as a resource.
Start and end offsets are inclusive.
Once a data-blob resource has been created, the underlying bytes are held by the blobstore service for the lifetime
of the data-blob resource, even if the object they came from is later deleted.</p>
<h5>Params</h5>
<ul>
<li><a id="method_container_get_data.self"></a><code>self</code>: borrow&lt;<a href="#container"><a href="#container"><code>container</code></a></a>&gt;</li>
<li><a id="method_container_get_data.name"></a><code>name</code>: <a href="#object_name"><a href="#object_name"><code>object-name</code></a></a></li>
<li><a id="method_container_get_data.start"></a><code>start</code>: <code>u64</code></li>
<li><a id="method_container_get_data.end"></a><code>end</code>: <code>u64</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_container_get_data.0"></a> result&lt;own&lt;<a href="#incoming_value"><a href="#incoming_value"><code>incoming-value</code></a></a>&gt;, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_container_write_data"></a><code>[method]container.write-data: func</code></h4>
<p>creates or replaces an object with the data blob.</p>
<h5>Params</h5>
<ul>
<li><a id="method_container_write_data.self"></a><code>self</code>: borrow&lt;<a href="#container"><a href="#container"><code>container</code></a></a>&gt;</li>
<li><a id="method_container_write_data.name"></a><code>name</code>: <a href="#object_name"><a href="#object_name"><code>object-name</code></a></a></li>
<li><a id="method_container_write_data.data"></a><code>data</code>: borrow&lt;<a href="#outgoing_value"><a href="#outgoing_value"><code>outgoing-value</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_container_write_data.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_container_list_objects"></a><code>[method]container.list-objects: func</code></h4>
<p>returns list of objects in the container. Order is undefined.</p>
<h5>Params</h5>
<ul>
<li><a id="method_container_list_objects.self"></a><code>self</code>: borrow&lt;<a href="#container"><a href="#container"><code>container</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_container_list_objects.0"></a> result&lt;own&lt;<a href="#stream_object_names"><a href="#stream_object_names"><code>stream-object-names</code></a></a>&gt;, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_container_delete_object"></a><code>[method]container.delete-object: func</code></h4>
<p>deletes object.
does not return error if object did not exist.</p>
<h5>Params</h5>
<ul>
<li><a id="method_container_delete_object.self"></a><code>self</code>: borrow&lt;<a href="#container"><a href="#container"><code>container</code></a></a>&gt;</li>
<li><a id="method_container_delete_object.name"></a><code>name</code>: <a href="#object_name"><a href="#object_name"><code>object-name</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_container_delete_object.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_container_delete_objects"></a><code>[method]container.delete-objects: func</code></h4>
<p>deletes multiple objects in the container</p>
<h5>Params</h5>
<ul>
<li><a id="method_container_delete_objects.self"></a><code>self</code>: borrow&lt;<a href="#container"><a href="#container"><code>container</code></a></a>&gt;</li>
<li><a id="method_container_delete_objects.names"></a><code>names</code>: list&lt;<a href="#object_name"><a href="#object_name"><code>object-name</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_container_delete_objects.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_container_has_object"></a><code>[method]container.has-object: func</code></h4>
<p>returns true if the object exists in this container</p>
<h5>Params</h5>
<ul>
<li><a id="method_container_has_object.self"></a><code>self</code>: borrow&lt;<a href="#container"><a href="#container"><code>container</code></a></a>&gt;</li>
<li><a id="method_container_has_object.name"></a><code>name</code>: <a href="#object_name"><a href="#object_name"><code>object-name</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_container_has_object.0"></a> result&lt;<code>bool</code>, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_container_object_info"></a><code>[method]container.object-info: func</code></h4>
<p>returns metadata for the object</p>
<h5>Params</h5>
<ul>
<li><a id="method_container_object_info.self"></a><code>self</code>: borrow&lt;<a href="#container"><a href="#container"><code>container</code></a></a>&gt;</li>
<li><a id="method_container_object_info.name"></a><code>name</code>: <a href="#object_name"><a href="#object_name"><code>object-name</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_container_object_info.0"></a> result&lt;<a href="#object_metadata"><a href="#object_metadata"><code>object-metadata</code></a></a>, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_container_clear"></a><code>[method]container.clear: func</code></h4>
<p>removes all objects within the container, leaving the container empty.</p>
<h5>Params</h5>
<ul>
<li><a id="method_container_clear.self"></a><code>self</code>: borrow&lt;<a href="#container"><a href="#container"><code>container</code></a></a>&gt;</li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_container_clear.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_stream_object_names_read_stream_object_names"></a><code>[method]stream-object-names.read-stream-object-names: func</code></h4>
<p>reads the next number of objects from the stream</p>
<p>This function returns the list of objects read, and a boolean indicating if the end of the stream was reached.</p>
<h5>Params</h5>
<ul>
<li><a id="method_stream_object_names_read_stream_object_names.self"></a><code>self</code>: borrow&lt;<a href="#stream_object_names"><a href="#stream_object_names"><code>stream-object-names</code></a></a>&gt;</li>
<li><a id="method_stream_object_names_read_stream_object_names.len"></a><code>len</code>: <code>u64</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_stream_object_names_read_stream_object_names.0"></a> result&lt;(list&lt;<a href="#object_name"><a href="#object_name"><code>object-name</code></a></a>&gt;, <code>bool</code>), <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="method_stream_object_names_skip_stream_object_names"></a><code>[method]stream-object-names.skip-stream-object-names: func</code></h4>
<p>skip the next number of objects in the stream</p>
<p>This function returns the number of objects skipped, and a boolean indicating if the end of the stream was reached.</p>
<h5>Params</h5>
<ul>
<li><a id="method_stream_object_names_skip_stream_object_names.self"></a><code>self</code>: borrow&lt;<a href="#stream_object_names"><a href="#stream_object_names"><code>stream-object-names</code></a></a>&gt;</li>
<li><a id="method_stream_object_names_skip_stream_object_names.num"></a><code>num</code>: <code>u64</code></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="method_stream_object_names_skip_stream_object_names.0"></a> result&lt;(<code>u64</code>, <code>bool</code>), <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h2><a id="wasi_blobstore_blobstore_0_2_0_draft"></a>Import interface wasi:blobstore/blobstore@0.2.0-draft</h2>
<p>wasi-cloud Blobstore service definition</p>
<hr />
<h3>Types</h3>
<h4><a id="container"></a><code>type container</code></h4>
<p><a href="#container"><a href="#container"><code>container</code></a></a></p>
<p>
#### <a id="error"></a>`type error`
[`error`](#error)
<p>
#### <a id="container_name"></a>`type container-name`
[`container-name`](#container_name)
<p>
#### <a id="object_id"></a>`type object-id`
[`object-id`](#object_id)
<p>
----
<h3>Functions</h3>
<h4><a id="create_container"></a><code>create-container: func</code></h4>
<p>creates a new empty container</p>
<h5>Params</h5>
<ul>
<li><a id="create_container.name"></a><code>name</code>: <a href="#container_name"><a href="#container_name"><code>container-name</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="create_container.0"></a> result&lt;own&lt;<a href="#container"><a href="#container"><code>container</code></a></a>&gt;, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="get_container"></a><code>get-container: func</code></h4>
<p>retrieves a container by name</p>
<h5>Params</h5>
<ul>
<li><a id="get_container.name"></a><code>name</code>: <a href="#container_name"><a href="#container_name"><code>container-name</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="get_container.0"></a> result&lt;own&lt;<a href="#container"><a href="#container"><code>container</code></a></a>&gt;, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="delete_container"></a><code>delete-container: func</code></h4>
<p>deletes a container and all objects within it</p>
<h5>Params</h5>
<ul>
<li><a id="delete_container.name"></a><code>name</code>: <a href="#container_name"><a href="#container_name"><code>container-name</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="delete_container.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="container_exists"></a><code>container-exists: func</code></h4>
<p>returns true if the container exists</p>
<h5>Params</h5>
<ul>
<li><a id="container_exists.name"></a><code>name</code>: <a href="#container_name"><a href="#container_name"><code>container-name</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="container_exists.0"></a> result&lt;<code>bool</code>, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="copy_object"></a><code>copy-object: func</code></h4>
<p>copies (duplicates) an object, to the same or a different container.
returns an error if the target container does not exist.
overwrites destination object if it already existed.</p>
<h5>Params</h5>
<ul>
<li><a id="copy_object.src"></a><code>src</code>: <a href="#object_id"><a href="#object_id"><code>object-id</code></a></a></li>
<li><a id="copy_object.dest"></a><code>dest</code>: <a href="#object_id"><a href="#object_id"><code>object-id</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="copy_object.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>
<h4><a id="move_object"></a><code>move-object: func</code></h4>
<p>moves or renames an object, to the same or a different container
returns an error if the destination container does not exist.
overwrites destination object if it already existed.</p>
<h5>Params</h5>
<ul>
<li><a id="move_object.src"></a><code>src</code>: <a href="#object_id"><a href="#object_id"><code>object-id</code></a></a></li>
<li><a id="move_object.dest"></a><code>dest</code>: <a href="#object_id"><a href="#object_id"><code>object-id</code></a></a></li>
</ul>
<h5>Return values</h5>
<ul>
<li><a id="move_object.0"></a> result&lt;_, <a href="#error"><a href="#error"><code>error</code></a></a>&gt;</li>
</ul>

# Evidence-Led Debug

## Classify and locate the current failure

Identify the failing stage: setup, compile, elaboration, execution, checking,
timeout, or reporting. Establish the current run's identity, configuration,
test/seed, log paths, and whether execution actually reached that stage.
Do not infer simulation success from successful submission or an old PASS line.

## Compilation and elaboration

Read the current compile/elaboration log first. Locate the first meaningful
causal diagnostic with enough surrounding context, not just the final wrapper
error or a flood of downstream messages. Follow its file/line, include path,
package dependency, define, type, or configuration evidence.

Inspect flow/build scripts only when the diagnostic implicates command construction,
file lists, generated inputs, tool selection, environment, or configuration, or
when a narrowly scoped search is needed to locate the missing log. Do not read
unrelated scripts or directories as a default response to compilation failure.

## Functional mismatch

Start from the first relevant mismatch, transaction ID and timestamp. Reconstruct:
configuration -> accepted input -> expected result -> DUT output -> monitor
transaction -> comparison. Consider DUT, stimulus, predictor, monitor and matching
logic as possible causes. Do not assume either RTL or the scoreboard is correct.

Use waveforms only for the necessary signals/time window and expand based on
evidence. Trace the earliest divergence rather than only its final symptom.

## Timeout, hang and regression

For hangs, determine the last confirmed progress and outstanding work: handshake,
response, credit, reset release, sequence synchronization, objection, queue drain,
or scheduler/tool completion. Do not simply enlarge timeouts without a justified
latency bound and a supported explanation.

Group regression failures by causal signature and configuration before selecting
representative runs. Similar messages do not prove identical causes. Preserve
the distinction between infrastructure failures and functional failures.

## Hypothesis discipline and stopping

Keep a small set of hypotheses, each with evidence and a discriminating next
check. Prefer the narrowest read or experiment that can reject one. Repeating an
unchanged failing command is not a new experiment. When checks stop adding
evidence, report what is missing instead of broadening into unrelated exploration.

Return confirmed observations, supported root cause or remaining hypotheses,
affected scope, and proposed fix. Implement only when authorized. After a fix,
rerun the reproduction and relevant regression; diagnosis alone is not validation.

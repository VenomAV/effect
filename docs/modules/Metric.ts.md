---
title: Metric.ts
nav_order: 57
parent: Modules
---

## Metric overview

Added in v2.0.0

---

<h2 class="text-delta">Table of contents</h2>

- [aspects](#aspects)
  - [increment](#increment)
  - [incrementBy](#incrementby)
  - [set](#set)
  - [trackAll](#trackall)
  - [trackDefect](#trackdefect)
  - [trackDefectWith](#trackdefectwith)
  - [trackDuration](#trackduration)
  - [trackDurationWith](#trackdurationwith)
  - [trackError](#trackerror)
  - [trackErrorWith](#trackerrorwith)
  - [trackSuccess](#tracksuccess)
  - [trackSuccessWith](#tracksuccesswith)
- [constructors](#constructors)
  - [counter](#counter)
  - [frequency](#frequency)
  - [fromMetricKey](#frommetrickey)
  - [gauge](#gauge)
  - [histogram](#histogram)
  - [make](#make)
  - [succeed](#succeed)
  - [summary](#summary)
  - [summaryTimestamp](#summarytimestamp)
  - [sync](#sync)
  - [timer](#timer)
  - [timerWithBoundaries](#timerwithboundaries)
  - [withConstantInput](#withconstantinput)
- [getters](#getters)
  - [snapshot](#snapshot)
  - [value](#value)
- [globals](#globals)
  - [globalMetricRegistry](#globalmetricregistry)
- [mapping](#mapping)
  - [map](#map)
  - [mapInput](#mapinput)
  - [mapType](#maptype)
- [metrics](#metrics)
  - [fiberActive](#fiberactive)
  - [fiberFailures](#fiberfailures)
  - [fiberLifetimes](#fiberlifetimes)
  - [fiberStarted](#fiberstarted)
  - [fiberSuccesses](#fibersuccesses)
- [models](#models)
  - [Metric (interface)](#metric-interface)
  - [MetricApply (interface)](#metricapply-interface)
- [symbols](#symbols)
  - [MetricTypeId](#metrictypeid)
  - [MetricTypeId (type alias)](#metrictypeid-type-alias)
- [unsafe](#unsafe)
  - [unsafeSnapshot](#unsafesnapshot)
- [utils](#utils)
  - [Metric (namespace)](#metric-namespace)
    - [Counter (interface)](#counter-interface)
    - [Frequency (interface)](#frequency-interface)
    - [Gauge (interface)](#gauge-interface)
    - [Histogram (interface)](#histogram-interface)
    - [Summary (interface)](#summary-interface)
    - [Variance (interface)](#variance-interface)
  - [tagged](#tagged)
  - [taggedWithLabels](#taggedwithlabels)
  - [taggedWithLabelsInput](#taggedwithlabelsinput)
  - [update](#update)
  - [withNow](#withnow)
- [zipping](#zipping)
  - [zip](#zip)

---

# aspects

## increment

**Signature**

```ts
export declare const increment: (
  self: Metric.Counter<number> | Metric.Counter<bigint>
) => Effect.Effect<never, never, void>
```

Added in v2.0.0

## incrementBy

**Signature**

```ts
export declare const incrementBy: {
  (amount: number): (self: Metric.Counter<number>) => Effect.Effect<never, never, void>
  (amount: bigint): (self: Metric.Counter<bigint>) => Effect.Effect<never, never, void>
  (self: Metric.Counter<number>, amount: number): Effect.Effect<never, never, void>
  (self: Metric.Counter<bigint>, amount: bigint): Effect.Effect<never, never, void>
}
```

Added in v2.0.0

## set

**Signature**

```ts
export declare const set: {
  (value: number): (self: Metric.Gauge<number>) => Effect.Effect<never, never, void>
  (value: bigint): (self: Metric.Gauge<bigint>) => Effect.Effect<never, never, void>
  (self: Metric.Gauge<number>, value: number): Effect.Effect<never, never, void>
  (self: Metric.Gauge<bigint>, value: bigint): Effect.Effect<never, never, void>
}
```

Added in v2.0.0

## trackAll

Returns an aspect that will update this metric with the specified constant
value every time the aspect is applied to an effect, regardless of whether
that effect fails or succeeds.

**Signature**

```ts
export declare const trackAll: {
  <In>(input: In): <Type, Out>(
    self: Metric<Type, In, Out>
  ) => <R, E, A>(effect: Effect.Effect<R, E, A>) => Effect.Effect<R, E, A>
  <Type, In, Out>(self: Metric<Type, In, Out>, input: In): <R, E, A>(
    effect: Effect.Effect<R, E, A>
  ) => Effect.Effect<R, E, A>
}
```

Added in v2.0.0

## trackDefect

Returns an aspect that will update this metric with the defects of the
effects that it is applied to.

**Signature**

```ts
export declare const trackDefect: {
  <Type, Out>(metric: Metric<Type, unknown, Out>): <R, E, A>(self: Effect.Effect<R, E, A>) => Effect.Effect<R, E, A>
  <R, E, A, Type, Out>(self: Effect.Effect<R, E, A>, metric: Metric<Type, unknown, Out>): Effect.Effect<R, E, A>
}
```

Added in v2.0.0

## trackDefectWith

Returns an aspect that will update this metric with the result of applying
the specified function to the defect throwables of the effects that the
aspect is applied to.

**Signature**

```ts
export declare const trackDefectWith: {
  <Type, In, Out>(metric: Metric<Type, In, Out>, f: (defect: unknown) => In): <R, E, A>(
    self: Effect.Effect<R, E, A>
  ) => Effect.Effect<R, E, A>
  <R, E, A, Type, In, Out>(
    self: Effect.Effect<R, E, A>,
    metric: Metric<Type, In, Out>,
    f: (defect: unknown) => In
  ): Effect.Effect<R, E, A>
}
```

Added in v2.0.0

## trackDuration

Returns an aspect that will update this metric with the duration that the
effect takes to execute. To call this method, the input type of the metric
must be `Duration`.

**Signature**

```ts
export declare const trackDuration: {
  <Type, Out>(metric: Metric<Type, Duration.Duration, Out>): <R, E, A>(
    self: Effect.Effect<R, E, A>
  ) => Effect.Effect<R, E, A>
  <R, E, A, Type, Out>(self: Effect.Effect<R, E, A>, metric: Metric<Type, Duration.Duration, Out>): Effect.Effect<
    R,
    E,
    A
  >
}
```

Added in v2.0.0

## trackDurationWith

Returns an aspect that will update this metric with the duration that the
effect takes to execute. To call this method, you must supply a function
that can convert the `Duration` to the input type of this metric.

**Signature**

```ts
export declare const trackDurationWith: {
  <Type, In, Out>(metric: Metric<Type, In, Out>, f: (duration: Duration.Duration) => In): <R, E, A>(
    effect: Effect.Effect<R, E, A>
  ) => Effect.Effect<R, E, A>
  <R, E, A, Type, In, Out>(
    self: Effect.Effect<R, E, A>,
    metric: Metric<Type, In, Out>,
    f: (duration: Duration.Duration) => In
  ): Effect.Effect<R, E, A>
}
```

Added in v2.0.0

## trackError

Returns an aspect that will update this metric with the failure value of
the effects that it is applied to.

**Signature**

```ts
export declare const trackError: {
  <Type, In, Out>(metric: Metric<Type, In, Out>): <R, E extends In, A>(
    self: Effect.Effect<R, E, A>
  ) => Effect.Effect<R, E, A>
  <R, E extends In, A, Type, In, Out>(self: Effect.Effect<R, E, A>, metric: Metric<Type, In, Out>): Effect.Effect<
    R,
    E,
    A
  >
}
```

Added in v2.0.0

## trackErrorWith

Returns an aspect that will update this metric with the result of applying
the specified function to the error value of the effects that the aspect is
applied to.

**Signature**

```ts
export declare const trackErrorWith: {
  <Type, In, Out, In2>(metric: Metric<Type, In, Out>, f: (error: In2) => In): <R, E extends In2, A>(
    effect: Effect.Effect<R, E, A>
  ) => Effect.Effect<R, E, A>
  <R, E extends In2, A, Type, In, Out, In2>(
    self: Effect.Effect<R, E, A>,
    metric: Metric<Type, In, Out>,
    f: (error: In2) => In
  ): Effect.Effect<R, E, A>
}
```

Added in v2.0.0

## trackSuccess

Returns an aspect that will update this metric with the success value of
the effects that it is applied to.

**Signature**

```ts
export declare const trackSuccess: {
  <Type, In, Out>(metric: Metric<Type, In, Out>): <R, E, A extends In>(
    self: Effect.Effect<R, E, A>
  ) => Effect.Effect<R, E, A>
  <R, E, A extends In, Type, In, Out>(self: Effect.Effect<R, E, A>, metric: Metric<Type, In, Out>): Effect.Effect<
    R,
    E,
    A
  >
}
```

Added in v2.0.0

## trackSuccessWith

Returns an aspect that will update this metric with the result of applying
the specified function to the success value of the effects that the aspect is
applied to.

**Signature**

```ts
export declare const trackSuccessWith: {
  <Type, In, Out, In2>(metric: Metric<Type, In, Out>, f: (value: In2) => In): <R, E, A extends In2>(
    self: Effect.Effect<R, E, A>
  ) => Effect.Effect<R, E, A>
  <R, E, A extends In2, Type, In, Out, In2>(
    self: Effect.Effect<R, E, A>,
    metric: Metric<Type, In, Out>,
    f: (value: In2) => In
  ): Effect.Effect<R, E, A>
}
```

Added in v2.0.0

# constructors

## counter

A counter, which can be incremented by numbers.

**Signature**

```ts
export declare const counter: {
  (
    name: string,
    options?: { readonly description?: string; readonly bigint?: false; readonly incremental?: boolean }
  ): Metric.Counter<number>
  (
    name: string,
    options: { readonly description?: string; readonly bigint: true; readonly incremental?: boolean }
  ): Metric.Counter<bigint>
}
```

Added in v2.0.0

## frequency

A string histogram metric, which keeps track of the counts of different
strings.

**Signature**

```ts
export declare const frequency: (name: string, description?: string) => Metric.Frequency<string>
```

Added in v2.0.0

## fromMetricKey

**Signature**

```ts
export declare const fromMetricKey: <Type extends MetricKeyType.MetricKeyType<any, any>>(
  key: MetricKey.MetricKey<Type>
) => Metric<Type, MetricKeyType.MetricKeyType.InType<Type>, MetricKeyType.MetricKeyType.OutType<Type>>
```

Added in v2.0.0

## gauge

A gauge, which can be set to a value.

**Signature**

```ts
export declare const gauge: {
  (name: string, options?: { readonly description?: string; readonly bigint?: false }): Metric.Gauge<number>
  (name: string, options: { readonly description?: string; readonly bigint: true }): Metric.Gauge<bigint>
}
```

Added in v2.0.0

## histogram

A numeric histogram metric, which keeps track of the count of numbers that
fall in bins with the specified boundaries.

**Signature**

```ts
export declare const histogram: (
  name: string,
  boundaries: MetricBoundaries.MetricBoundaries,
  description?: string
) => Metric<MetricKeyType.MetricKeyType.Histogram, number, MetricState.MetricState.Histogram>
```

Added in v2.0.0

## make

**Signature**

```ts
export declare const make: MetricApply
```

Added in v2.0.0

## succeed

Creates a metric that ignores input and produces constant output.

**Signature**

```ts
export declare const succeed: <Out>(out: Out) => Metric<void, unknown, Out>
```

Added in v2.0.0

## summary

**Signature**

```ts
export declare const summary: (options: {
  readonly name: string
  readonly maxAge: Duration.DurationInput
  readonly maxSize: number
  readonly error: number
  readonly quantiles: Chunk.Chunk<number>
  readonly description?: string
}) => Metric.Summary<number>
```

Added in v2.0.0

## summaryTimestamp

**Signature**

```ts
export declare const summaryTimestamp: (options: {
  readonly name: string
  readonly maxAge: Duration.DurationInput
  readonly maxSize: number
  readonly error: number
  readonly quantiles: Chunk.Chunk<number>
  readonly description?: string
}) => Metric.Summary<readonly [value: number, timestamp: number]>
```

Added in v2.0.0

## sync

Creates a metric that ignores input and produces constant output.

**Signature**

```ts
export declare const sync: <Out>(evaluate: LazyArg<Out>) => Metric<void, unknown, Out>
```

Added in v2.0.0

## timer

Creates a timer metric, based on a histogram, which keeps track of
durations in milliseconds. The unit of time will automatically be added to
the metric as a tag (i.e. `"time_unit: milliseconds"`).

**Signature**

```ts
export declare const timer: (
  name: string
) => Metric<MetricKeyType.MetricKeyType.Histogram, Duration.Duration, MetricState.MetricState.Histogram>
```

Added in v2.0.0

## timerWithBoundaries

Creates a timer metric, based on a histogram created from the provided
boundaries, which keeps track of durations in milliseconds. The unit of time
will automatically be added to the metric as a tag (i.e.
`"time_unit: milliseconds"`).

**Signature**

```ts
export declare const timerWithBoundaries: (
  name: string,
  boundaries: Chunk.Chunk<number>
) => Metric<MetricKeyType.MetricKeyType.Histogram, Duration.Duration, MetricState.MetricState.Histogram>
```

Added in v2.0.0

## withConstantInput

Returns a new metric that is powered by this one, but which accepts updates
of any type, and translates them to updates with the specified constant
update value.

**Signature**

```ts
export declare const withConstantInput: {
  <In>(input: In): <Type, Out>(self: Metric<Type, In, Out>) => Metric<Type, unknown, Out>
  <Type, In, Out>(self: Metric<Type, In, Out>, input: In): Metric<Type, unknown, Out>
}
```

Added in v2.0.0

# getters

## snapshot

Captures a snapshot of all metrics recorded by the application.

**Signature**

```ts
export declare const snapshot: Effect.Effect<never, never, HashSet.HashSet<MetricPair.MetricPair.Untyped>>
```

Added in v2.0.0

## value

Retrieves a snapshot of the value of the metric at this moment in time.

**Signature**

```ts
export declare const value: <Type, In, Out>(self: Metric<Type, In, Out>) => Effect.Effect<never, never, Out>
```

Added in v2.0.0

# globals

## globalMetricRegistry

**Signature**

```ts
export declare const globalMetricRegistry: MetricRegistry.MetricRegistry
```

Added in v2.0.0

# mapping

## map

Returns a new metric that is powered by this one, but which outputs a new
state type, determined by transforming the state type of this metric by the
specified function.

**Signature**

```ts
export declare const map: {
  <Out, Out2>(f: (out: Out) => Out2): <Type, In>(self: Metric<Type, In, Out>) => Metric<Type, In, Out2>
  <Type, In, Out, Out2>(self: Metric<Type, In, Out>, f: (out: Out) => Out2): Metric<Type, In, Out2>
}
```

Added in v2.0.0

## mapInput

Returns a new metric that is powered by this one, but which accepts updates
of the specified new type, which must be transformable to the input type of
this metric.

**Signature**

```ts
export declare const mapInput: {
  <In, In2>(f: (input: In2) => In): <Type, Out>(self: Metric<Type, In, Out>) => Metric<Type, In2, Out>
  <Type, In, Out, In2>(self: Metric<Type, In, Out>, f: (input: In2) => In): Metric<Type, In2, Out>
}
```

Added in v2.0.0

## mapType

**Signature**

```ts
export declare const mapType: {
  <Type, Type2>(f: (type: Type) => Type2): <In, Out>(self: Metric<Type, In, Out>) => Metric<Type2, In, Out>
  <Type, In, Out, Type2>(self: Metric<Type, In, Out>, f: (type: Type) => Type2): Metric<Type2, In, Out>
}
```

Added in v2.0.0

# metrics

## fiberActive

**Signature**

```ts
export declare const fiberActive: Metric.Counter<number>
```

Added in v2.0.0

## fiberFailures

**Signature**

```ts
export declare const fiberFailures: Metric.Counter<number>
```

Added in v2.0.0

## fiberLifetimes

**Signature**

```ts
export declare const fiberLifetimes: Metric<
  MetricKeyType.MetricKeyType.Histogram,
  number,
  MetricState.MetricState.Histogram
>
```

Added in v2.0.0

## fiberStarted

**Signature**

```ts
export declare const fiberStarted: Metric.Counter<number>
```

Added in v2.0.0

## fiberSuccesses

**Signature**

```ts
export declare const fiberSuccesses: Metric.Counter<number>
```

Added in v2.0.0

# models

## Metric (interface)

A `Metric<Type, In, Out>` represents a concurrent metric which accepts
updates of type `In` and are aggregated to a stateful value of type `Out`.

For example, a counter metric would have type `Metric<number, number>`,
representing the fact that the metric can be updated with numbers (the amount
to increment or decrement the counter by), and the state of the counter is a
number.

There are five primitive metric types supported by Effect:

- Counters
- Frequencies
- Gauges
- Histograms
- Summaries

**Signature**

```ts
export interface Metric<Type, In, Out> extends Metric.Variance<Type, In, Out>, Pipeable {
  /**
   * The type of the underlying primitive metric. For example, this could be
   * `MetricKeyType.Counter` or `MetricKeyType.Gauge`.
   */
  readonly keyType: Type
  readonly unsafeUpdate: (input: In, extraTags: HashSet.HashSet<MetricLabel.MetricLabel>) => void
  readonly unsafeValue: (extraTags: HashSet.HashSet<MetricLabel.MetricLabel>) => Out
  /** */
  <R, E, A extends In>(effect: Effect.Effect<R, E, A>): Effect.Effect<R, E, A>
}
```

Added in v2.0.0

## MetricApply (interface)

**Signature**

```ts
export interface MetricApply {
  <Type, In, Out>(
    keyType: Type,
    unsafeUpdate: (input: In, extraTags: HashSet.HashSet<MetricLabel.MetricLabel>) => void,
    unsafeValue: (extraTags: HashSet.HashSet<MetricLabel.MetricLabel>) => Out
  ): Metric<Type, In, Out>
}
```

Added in v2.0.0

# symbols

## MetricTypeId

**Signature**

```ts
export declare const MetricTypeId: typeof MetricTypeId
```

Added in v2.0.0

## MetricTypeId (type alias)

**Signature**

```ts
export type MetricTypeId = typeof MetricTypeId
```

Added in v2.0.0

# unsafe

## unsafeSnapshot

Unsafely captures a snapshot of all metrics recorded by the application.

**Signature**

```ts
export declare const unsafeSnapshot: (_: void) => HashSet.HashSet<MetricPair.MetricPair.Untyped>
```

Added in v2.0.0

# utils

## Metric (namespace)

Added in v2.0.0

### Counter (interface)

**Signature**

```ts
export interface Counter<In extends number | bigint>
  extends Metric<MetricKeyType.MetricKeyType.Counter<In>, In, MetricState.MetricState.Counter<In>> {}
```

Added in v2.0.0

### Frequency (interface)

**Signature**

```ts
export interface Frequency<In>
  extends Metric<MetricKeyType.MetricKeyType.Frequency, In, MetricState.MetricState.Frequency> {}
```

Added in v2.0.0

### Gauge (interface)

**Signature**

```ts
export interface Gauge<In extends number | bigint>
  extends Metric<MetricKeyType.MetricKeyType.Gauge<In>, In, MetricState.MetricState.Gauge<In>> {}
```

Added in v2.0.0

### Histogram (interface)

**Signature**

```ts
export interface Histogram<In>
  extends Metric<MetricKeyType.MetricKeyType.Histogram, In, MetricState.MetricState.Histogram> {}
```

Added in v2.0.0

### Summary (interface)

**Signature**

```ts
export interface Summary<In> extends Metric<MetricKeyType.MetricKeyType.Summary, In, MetricState.MetricState.Summary> {}
```

Added in v2.0.0

### Variance (interface)

**Signature**

```ts
export interface Variance<Type, In, Out> {
  readonly [MetricTypeId]: {
    readonly _Type: (_: Type) => Type
    readonly _In: (_: In) => void
    readonly _Out: (_: never) => Out
  }
}
```

Added in v2.0.0

## tagged

Returns a new metric, which is identical in every way to this one, except
the specified tags have been added to the tags of this metric.

**Signature**

```ts
export declare const tagged: {
  <Type, In, Out>(key: string, value: string): (self: Metric<Type, In, Out>) => Metric<Type, In, Out>
  <Type, In, Out>(self: Metric<Type, In, Out>, key: string, value: string): Metric<Type, In, Out>
}
```

Added in v2.0.0

## taggedWithLabels

Returns a new metric, which is identical in every way to this one, except
the specified tags have been added to the tags of this metric.

**Signature**

```ts
export declare const taggedWithLabels: {
  <Type, In, Out>(extraTags: Iterable<MetricLabel.MetricLabel>): (self: Metric<Type, In, Out>) => Metric<Type, In, Out>
  <Type, In, Out>(self: Metric<Type, In, Out>, extraTags: Iterable<MetricLabel.MetricLabel>): Metric<Type, In, Out>
}
```

Added in v2.0.0

## taggedWithLabelsInput

Returns a new metric, which is identical in every way to this one, except
dynamic tags are added based on the update values. Note that the metric
returned by this method does not return any useful information, due to the
dynamic nature of the added tags.

**Signature**

```ts
export declare const taggedWithLabelsInput: {
  <In>(f: (input: In) => Iterable<MetricLabel.MetricLabel>): <Type, Out>(
    self: Metric<Type, In, Out>
  ) => Metric<Type, In, void>
  <Type, In, Out>(self: Metric<Type, In, Out>, f: (input: In) => Iterable<MetricLabel.MetricLabel>): Metric<
    Type,
    In,
    void
  >
}
```

Added in v2.0.0

## update

Updates the metric with the specified update message. For example, if the
metric were a counter, the update would increment the method by the
provided amount.

**Signature**

```ts
export declare const update: {
  <In>(input: In): <Type, Out>(self: Metric<Type, In, Out>) => Effect.Effect<never, never, void>
  <Type, In, Out>(self: Metric<Type, In, Out>, input: In): Effect.Effect<never, never, void>
}
```

Added in v2.0.0

## withNow

**Signature**

```ts
export declare const withNow: <Type, In, Out>(self: Metric<Type, readonly [In, number], Out>) => Metric<Type, In, Out>
```

Added in v2.0.0

# zipping

## zip

**Signature**

```ts
export declare const zip: {
  <Type2, In2, Out2>(that: Metric<Type2, In2, Out2>): <Type, In, Out>(
    self: Metric<Type, In, Out>
  ) => Metric<readonly [Type, Type2], readonly [In, In2], readonly [Out, Out2]>
  <Type, In, Out, Type2, In2, Out2>(self: Metric<Type, In, Out>, that: Metric<Type2, In2, Out2>): Metric<
    readonly [Type, Type2],
    readonly [In, In2],
    readonly [Out, Out2]
  >
}
```

Added in v2.0.0

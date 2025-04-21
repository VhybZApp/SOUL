# Example-Based Prompt Engineering and Response Guidelines

## Example: Utility Function for Formatting Time
```tsx file="utils/formatTime.ts"
export function formatTime(time: number): string {
  const minutes = Math.floor(time / 60000);
  const seconds = Math.floor((time % 60000) / 1000);
  const milliseconds = Math.floor((time % 1000) / 10);
  return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}.${milliseconds.toString().padStart(2, '0')}`;
}
```

## Example: Custom Hook for Stopwatch Logic
```tsx file="hooks/useStopwatch.ts"
import { useState, useRef, useCallback } from 'react';

export function useStopwatch(initialTime: number = 0) {
  const [time, setTime] = useState(initialTime);
  const [isRunning, setIsRunning] = useState(false);
  const intervalRef = useRef<NodeJS.Timeout | null>(null);

  const start = useCallback(() => {
    if (!isRunning) {
      setIsRunning(true);
      const startTime = Date.now() - time;
      intervalRef.current = setInterval(() => {
        setTime(Date.now() - startTime);
      }, 10);
    }
  }, [isRunning, time]);

  const pause = useCallback(() => {
    if (isRunning && intervalRef.current) {
      clearInterval(intervalRef.current);
      setIsRunning(false);
    }
  }, [isRunning]);

  const reset = useCallback(() => {
    if (intervalRef.current) {
      clearInterval(intervalRef.current);
    }
    setTime(0);
    setIsRunning(false);
  }, []);

  return { time, isRunning, start, pause, reset };
}
```

## Example: Types File for Component Props
```tsx file="types/stopwatch.ts"
export interface StopwatchProps {
  initialTime?: number;
}
```

## Example: Stopwatch React Component
```tsx file="stopwatch.tsx"
'use client'

import { Button } from '@/components/ui/button'
import { CardContent, Card } from '@/components/ui/card'
import { useStopwatch } from '../hooks/useStopwatch'
import { formatTime } from '../utils/formatTime'
import { StopwatchProps } from '../types/stopwatch'

export default function Stopwatch({ initialTime = 0 }: StopwatchProps) {
  const { time, isRunning, start, pause, reset } = useStopwatch(initialTime);

  return (
    <Card className="w-full max-w-md">
      <CardContent className="flex flex-col items-center justify-center gap-4 p-4">
        <div 
          className="text-6xl font-bold tabular-nums" 
          aria-live="polite"
          aria-atomic="true"
        >
          {formatTime(time)}
        </div>
        <div className="flex gap-4">
          <Button 
            onClick={isRunning ? pause : start}
            aria-pressed={isRunning}
          >
            {isRunning ? 'Pause' : 'Start'}
          </Button>
          <Button 
            onClick={reset}
            disabled={time === 0 && !isRunning}
          >
            Reset
          </Button>
        </div>
      </CardContent>
    </Card>
  )
}
```

## Example: Explaining Capabilities
- Clearly state what the AI can and cannot process (e.g., images, code, etc.).

## Example: Multi-File Project Creation
- Demonstrate how to create both server actions and client components in a modular, organized way.

## Example: General Code Block Usage
- Use general code blocks for concise answers or CLI commands when a full project/file is unnecessary.

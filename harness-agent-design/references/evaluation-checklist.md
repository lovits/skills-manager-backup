# Evaluation Checklist

## Purpose

Use this checklist before recommending a harness as production-worthy.

## Checklist

- Does the harness match the actual runtime shape?
- Can you explain why each subsystem exists?
- Is the loop visible and recoverable?
- Are tool approvals and policy boundaries explicit?
- Is memory justified and scoped by retention horizon?
- Is subagent or worker usage tied to context isolation needs?
- Is a gateway justified by channel or lifecycle complexity?
- Is there a restart and crash-recovery story?
- Is there an eval strategy for realistic tasks?
- Can the first version ship with less complexity?

# Skipping a GitHub Actions step without failing

I wanted to have a GitHub Action step run that might fail, but if it failed the rest of the steps should still execute and the overall run should be treated as a success.

`continue-on-error: true` does exactly that:

```yaml
    - name: Task 1
      run: task1
      continue-on-error: true
    - name: Task 2
      run: task2
```

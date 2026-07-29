# Automated Accessibility (A11y) Testing Frameworks

> Practical guide and code examples for integrating automated accessibility audits into Playwright, Cypress, Jest/Vitest, and Selenium test suites.

Automated accessibility testing allows teams to catch WCAG violations continuously in CI/CD pipelines before code is deployed to production.

---

## 1. Playwright + `@axe-core/playwright` (TypeScript / JavaScript)
* **Best For:** Modern E2E web automation pipelines using Playwright.
* **Installation:**
  ```bash
  npm install --save-dev @axe-core/playwright
  ```
* **Example Test (`tests/accessibility.spec.ts`):**
  ```typescript
  import { test, expect } from '@playwright/test';
  import AxeBuilder from '@axe-core/playwright';

  test.describe('Accessibility Audits', () => {
    test('should pass automated WCAG 2.1 AA checks on home page', async ({ page }) => {
      await page.goto('https://your-app.com');

      const accessibilityScanResults = await new AxeBuilder({ page })
        .withTags(['wcag2a', 'wcag2aa', 'wcag21a', 'wcag21aa'])
        .analyze();

      expect(accessibilityScanResults.violations).toEqual([]);
    });

    test('should audit specific component container (e.g., navigation modal)', async ({ page }) => {
      await page.goto('https://your-app.com');
      await page.click('#open-modal-btn');

      const modalScanResults = await new AxeBuilder({ page })
        .include('#modal-container')
        .analyze();

      expect(modalScanResults.violations).toEqual([]);
    });
  });
  ```

---

## 2. Cypress + `cypress-axe` (JavaScript / TypeScript)
* **Best For:** Web applications tested with Cypress E2E framework.
* **Installation:**
  ```bash
  npm install --save-dev cypress-axe axe-core
  ```
* **Example Test (`cypress/e2e/a11y.cy.js`):**
  ```javascript
  // Import in cypress/support/e2e.js: import 'cypress-axe'

  describe('Accessibility Audits with Cypress', () => {
    beforeEach(() => {
      cy.visit('https://your-app.com');
      cy.injectAxe(); // Inject axe-core engine into the page
    });

    it('should have no detectable accessibility violations on page load', () => {
      cy.checkA11y();
    });

    it('should audit specific element with custom rules', () => {
      cy.checkA11y('#main-content', {
        runOnly: {
          type: 'tag',
          values: ['wcag2aa']
        }
      });
    });
  });
  ```

---

## 3. Jest / Vitest + `jest-axe` (React / Component Unit Tests)
* **Best For:** Auditing React, Vue, or Svelte components in isolation during unit/component tests.
* **Installation:**
  ```bash
  npm install --save-dev jest-axe
  ```
* **Example Test (`Button.test.tsx`):**
  ```typescript
  import React from 'react';
  import { render } from '@testing-library/react';
  import { axe, toHaveNoViolations } from 'jest-axe';
  import { PrimaryButton } from './PrimaryButton';

  expect.extend(toHaveNoViolations);

  test('PrimaryButton component should have no accessibility violations', async () => {
    const { container } = render(<PrimaryButton label="Submit Form" />);
    const results = await axe(container);

    expect(results).toHaveNoViolations();
  });
  ```

---

## 4. Selenium + `axe-selenium-python` (Python)
* **Best For:** Python-based E2E test suites using Selenium WebDriver or Pytest.
* **Installation:**
  ```bash
  pip install axe-selenium-python
  ```
* **Example Test (`test_a11y.py`):**
  ```python
  from selenium import webdriver
  from axe_selenium_python import Axe

  def test_google_accessibility():
      driver = webdriver.Chrome()
      driver.get("https://your-app.com")

      axe = Axe(driver)
      axe.inject() # Inject axe-core engine
      results = axe.run()

      # Write results to JSON artifact
      axe.write_results(results, 'a11y-results.json')

      # Assert no violations found
      assert len(results["violations"]) == 0, f"Found {len(results['violations'])} accessibility violations"
      driver.quit()
  ```

---

## Summary
### Why `axe-core`?
* **100% Free & Open Source:** The `axe-core` engine and all code automation adapters (`@axe-core/playwright`, `cypress-axe`, `jest-axe`, `axe-selenium-python`) are free and open-source (MPL 2.0 license) for commercial and private test suites.
* **Industry Adoption:** Even Google Lighthouse uses `axe-core` under the hood for its accessibility audits.

### Open Source Alternatives
* **Pa11y / pa11y-ci:** Popular LGPL-3.0 CLI runner, ideal for standalone terminal scripts and CI build steps.
* **Alfa (by Siteimprove):** Modern MIT-licensed accessibility engine written in TypeScript.
* **HTML_CodeSniffer:** Classic open-source client-side script engine.

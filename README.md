// Playwright-PGA-test


import { test, expect } from "@playwright/test";

test.beforeEach(async ({ page }) => {

  // Navigate to the homepage with a custom timeout and wait condition
  await page.goto("https://www.pgatour.com", {
    timeout: 20000, // Extend timeout to 20 seconds
    waitUntil: "domcontentloaded", // Wait until DOM is fully loaded
  })

  // Abort resource-heavy requests
    await page.route("**/*.{png,jpg,jpeg,gif,svg,webp}", (route) =>
    route.abort())
})

test.describe("PGA Tour Website Regression Tests", () => {
  test("PGA Tour Homepage Loads Correctly", async ({ page }) => {
      
      // Verify the page title
      await expect(page).toHaveTitle(/PGA TOUR/)
      
      // Check if the logo is visible
      await page.locator('img[alt="PGA TOUR"]').first().click();
       })
  
  test("Navigation Links Work Correctly", async ({ page }) => {
      
      // Click on the "Leaderboard" link
      await page.locator('nav a:has-text("Leaderboard")').click();

      //Verify that the URL has changed
      await expect(page).toHaveURL(/.*leaderboard/);
    });

    test("Profile form and sign in", async ({ page }) => {
      await page.getByRole("button", { name: "Profile" }).click();
      await page.locator('div p:has-text("Sigh In")').isVisible()
      await page.getByRole("textbox", { name: "email" }).fill("kateusa2023@gmail.com")
      await page.getByRole("textbox", { name: "password" }).fill("Khonda3336")
      await page.getByRole("button",{name:"Sign In"}).click()
    });
})

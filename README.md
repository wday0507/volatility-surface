# Volatility Surface Constructor

## Summary
This project constructs a smooth volatility surface for options data from Yahoo Finance. K-Nearest Neighbours is used to detect outlier IV's, and a Radial Basis Function is used for 3D interpolation of the surface.


## Process

**1/ Data Collection** - collect options data from YF

**2/ Data Cleaning** - e.g. removing options that: - have ultra low IV
                                                      - have NA values
                                                      - are illiquid
                                                      - are extremely OTM/ITM

**3/ Outlier Detection** - moneyness and maturity data is normalised and neighbours are identified
                            - IVs that are more than X std deviations away from average are not considered in interpolation (this helps keep the surface smooth)

**4/ RBF Interpolation and plotting** - a meshgrid for interpolation from normalised moneyness and maturity data is constructed
                                         - the rbf interpolator calcualts model IV values at meshgrid points, meshgrid points are then rescaled
                                         - surface is plotted using Plotly's 3D Surface and Scatter classes




## Strengths & Limitations
